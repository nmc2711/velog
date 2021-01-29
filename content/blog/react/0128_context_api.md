---
title: '[ Ch # 02 ] Context Api 구조설계 '
date: 2021-01-28 18:00:00
category: 'react'
draft: false
---

## React 전역 상태관리, Context Api 구조 설계

> React 에서 컴포넌트 간의 데이터 전달 방식은 '위 -> 아래' props를 통해
> 전달 합니다. 이때 프로젝트의 규모가 크거나 depth가 깊어지면 상향식 또는 하향식 콜백
> 시스템에서 매우 혼란스러워 집니다.
> 따라서 React component Tree 내에서 데이터를 글로벌 하게 공유할 수 있도록 선보인
> 방법이 context 입니다. provider(컨텍스트를 사용하여 컴포넌트 트리에 데이터를 공급) 하거나
> consumer (컴포넌트들이 설정하거나 재설정한 데이터를) 사용할수 있습니다.

GlobalContext 를 구성해 봅시다.

```js
// context/global_ctx.tsx

import React, {createContext,useState} from "react";

type BundleType = {
  globalState: StateType;
  globalAction: ActionType;
};
type StateType = {
    globalNumber:number;
}
type ActionType = {
    setGlobalNumber?(data:number):void;
}
type initialData = {
    globalState : {
        globalNumber:0
    },
    globalAction: {}
}

//  React.createContext 전역적인 정보 값을 가진 context 객체를  생성하고 초기 값 설정.
const GlobalContext = createContext<BundleType>(initialData);

// GlobalContext 객체내 provider 요소를 사용해 전역으로 사용할 정보를 값(value)으로 설정.
function GlobalProvider(props: { children: JSX.Element }) {
  // 개념이 어려워 보여도 app.tsx 에서 usestate를 사용하여 state를 정의하고 하위 컴포넌트에 prop을 통한 드릴링과 callback을 하되
  // 모두 동등한 위치의 1depth의 컴포넌트라고 생각 하면 될거같다.

  //initial State
  const [globalNumber, setGlobalNumber] = useState<number>(initialData.globalState.globalNumber);

  const globalState: StateType = {
    globalNumber
  };

  const globalAction: ActionType = {
    setGlobalNumber
  };

  // provider value (state와 setState)
  const bundle = {
    globalState,
    globalAction,
  };

  return <GlobalContext.Provider value={bundle}>{props.children}</GlobalContext.Provider>;
}
// context객체와 provider를 다른 컴포넌트 트리에서 사용할 수 있게 export해준다.
export { GlobalContext, GlobalProvider };

```

전역적으로 사용하기 위한 context이므로 index.tsx 내에서 provider 호출후 감싸준다.

```js
// context/global_ctx.tsx
import React, { useContext } from 'react'
import { GlobalProvider } from 'context/global_ctx'

ReactDOM.render(
  <React.StrictMode>
    <GlobalProvider>
      <App />
    </GlobalProvider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

> Global Context를 사용해보자

```js
import React, { useContext, useState,useLayOutEffect } from 'react'
// 사용할 global context 객체를 import
import { GlobalContext } from 'context/global_ctx'
export default function ZooList() {
  // GlobalContext 내의 state와 action 을 구조분해 하여 쓰기 쉽게 가져온다.
  const { globalState, globalAction } = useContext(GlobalContext)

  const changeGlobalState = () => {
    //context에서 전역 data 업데이트 하기
    globalAction!.setGlobalNumber(7)
  }

  useLayOutEffect(() => {
    //context에서 전역 data 가져오기
    console.log(globalState.globalNumber)    // return 0
  }, []);
  useEffect (()=> {
    //버튼 온클릭 이벤트 실행시 변화된 출력값 확인
    if (globalState.globalNumber !== 0) {
    console.log(globalState.globalNumber)   // return 7
    }
  },[globalState.globalNumber])

  return <button onClick={changeGlobalState}>Click</button>
}
```
