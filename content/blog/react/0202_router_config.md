---
title: '[ Ch # 03 ] React Router 구조설계 및 정리 '
date: 2021-02-02 18:04:11
category: 'react'
draft: false
---

## 프로젝트 시작의 뼈대라고 할수있는 라우터 구조를 설계 해보려고 한다.

`라우팅`이란 ? URL 이동에 따른 뷰를 보여주기 위한 행위 이며 , 대표적인 모듈은 `react-router-dom`

### -Route 구조 설계할때 유의할 3가지 철칙 !

##### 1. 현재 URL에 맞는 컴포넌트를 렌더링 해야 한다.

##### 2. 페이지의 리로드 없이 페이지 이동이 가능한 네비게이션 기능이 있어야 한다.

##### 3. 사용자의 액션(뒤로가기,앞으로가기)등 에 의해 URL이 변경될 때 이를 감지하고 처리할 수 있어야 한다.

<br />

### \* construction

Route는 모든 하위 컴포넌트에 영향을 미치고 작용하므로 최상단 `index.tsx || app.tsx` 부분에 위치를 시킨다. <br />
큰 프로젝트 또는 복잡한 프로젝트 같은경우 Route 컴포넌트를 따로 분리하여 사용하는게 바람직할 것이다.

```js
//app.tsx
import React from 'react'
import RouteWrap from './routeWrap'

function App() {
  return (
    <React.Fragment>
      <RouteWrap />
    </React.Fragment>
  )
}
export default App
```

`<BrowserRouter>` 는 HTML5의 history API를 활용하여 UI를 업데이트 하며 정보를 변경한다.<br />
`<Route>` 는 요청받은 pathname ( /url ) 에 해당하는 컴포넌트를 렌더링 한다. <br />
`<Switch>` 는 path의 충돌이 일어나지 않게 Route들을 관리한다. <br />
Swtich 내부에 Route들을 넣으면 요청에 의해 매칭되는 Route들이 다수 있을 때에 제일 처음 매칭되는 Route만 선별하여 실행하기 때문에 충돌 오류를 방지해주며, Route간에 이동 시 발생할 수 있는 충돌도 막아준다.
path와 매칭되는 Route가 없을 때에 맨 밑에 default Route의 실행이 보장된다.(path 속성을 명시하지 않은 Route)

```js
//routeWrap.tsx
import React from 'react'
import { BrowserRouter, Route } from 'react-router-dom'
import { LastLocationProvider } from 'react-router-last-location'
// 이전 히스토리를 간편하게 가져올수 있는 lib (* 선택사항)

import RouterSwitch from './route_switch'
// 필자 같은경우는 라우트를 세분화할 컴포넌트를 따로 분리하여 컴포넌트 속에서 <Switch>를 이용하여 path 관리를 하였다.

export default function RouteWrap() {
  return (
    <BrowserRouter>
      <LastLocationProvider>
        <Route component={RouterSwitch} />
        // pathname을 지정하지않으면 명시한 컴포넌트가 반환되며 그 속에서
        // <Switch> 기능을 이용해 일치하는 pathname의 컴포넌트를 반환되는 성질을 이용.
      </LastLocationProvider>
    </BrowserRouter>
  )
}
```

```js
//route_switch.tsx
import React, {
  lazy,
  useCallback,
  useContext,
  useEffect,
  useState,
} from 'react'
import { Redirect, Route, Switch } from 'react-router-dom'

import Main from 'pages/main'

const delayLoad = (importComponent: any, fallbackComponent?: any) => {
  // React.lazy는 컴포넌트를 렌더링하는 시점에서 비동기적으로 로딩할 수 있게 해 주는 유틸 함수이다.
  const LazyComponent = lazy(async () => {
    return importComponent
  })
  return () => (
    // Suspense는 리액트 내장 컴포넌트로서 코드 스플리팅된 컴포넌트를 로딩하도록
    // 발동시킬 수 있고, 로딩이 끝나지 않았을 때 보여 줄 UI를 설정할 수 있다.
    // 따라서, 각각 다른 fallbackComponent를 넘겨준다면 해당 path마다 각각의
    // loading 화면을 구현가능하다.
    <React.Suspense fallback={fallbackComponent ? fallbackComponent : null}>
      <LazyComponent />
      // 로딩이 완료된다면 반환할 컴포넌트
    </React.Suspense>
  )
}

const Account = delayLoad(
  import('pages/account'),
  <div>account 컴포넌트 로딩중...</div>
)
//로딩이 발생함
const MyInfo = delayLoad(import('pages/myInfo'))
//로딩이 발생하지않음

export default function RouterSwitch(props: any) {
  const { pathname } = props.location
  const [location, setLocation] = useState(props.location)

  useEffect(() => {
    setLocation(props.location)
    // 새로고침이나 비정상적인 상황에서 location 유지를 위함
  }, [props.location])
  return (
    <>
      <Switch location={location}>
        <Route exact path="/" component={Main} />
        <Route path="/myInfo" component={MyInfo} />
        <Route exact path="/account" component={Account} />
        <Route exact path="/account/:category" component={Account} />
        <Redirect to={'/'} />
      </Switch>
    </>
  )
}
// exact는 path가 정확히 일치해야 컴포넌트를 랜더링
// account path 같은경우 (/:) 를 통한 params를 생성하여 2depth url을 구성할수있다.

// 앞에서 설명한 것과 같이 할당된 path가 없는 경우
// main으로 이동하는  Redirect 구현
```
