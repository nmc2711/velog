---
title: 'Higher Order Component'
date: 2022-03-15 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">Higher Order Component란?</h3>

고차 컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수 <br />

React docs: (https://ko.reactjs.org/docs/higher-order-components.html) <br />

HOC는 컴포넌트 로직의 재사용을 위해 사용하는 기술로, <br />
argument로 받은 컴포넌트를 특정 기능을 실행한 후 결과에 따라 리턴하는 함수이다. <br />

<h3 style="color:#0230">좀더 쉽게 이해해보기</h3>

만약, 글을쓰는 프로젝트를 진행하는데 Client에서 라우팅을 한다고 가정하고 <br />

메인, 로그인, 글쓰기 컴포넌트 등이 있을 거고, 권한에 따라서 라우팅을 허용할 경우와 다시 되돌리는 경우가 있을 것이다. <br />

만약 client가 로그인을 한 경우엔 로그인 페이지로 이동하려해도 다시 메인 페이지로 돌아와야 한다. <br />

로그인을 하지 않은 경우엔 글쓰기 페이지로 이동하려 할때 로그인 페이지를 거쳐서 가야할 것이다. <br />

그렇다면 그 권한을 확인하기 위해선 개발의 방식에 따라 cookie나 session등에 저장한 정보를 계속해서 불러와 확인하는 로직을 계속해서 구현할텐데 <br />

해당 로직을 HOC로 두게 되면, 개발의 중복을 막고 재사용을 할 수 있는 것이다! <br />

이 외에도 HOC는 굉장히 자주 쓰인다. <br />

```javascript
```

<h3 style="color:#0230">왜 사용할까?</h3>

일반적으로 react는 부모 컴포넌트가 렌더링되면 자식 컴포넌트가 렌더링되는 tree 구조를 가지고 있다. <br />

하지만 때때로 이런 tree구조가 불편함을 가져다주기도 한다. 분명 부모-자식 관계를 가지고 있지만 독립적인 위치에서 렌더링을 하면 훨씬 편리한 경우가 있다.<br />

대표적인 예로 modal은 부모 컴포넌트의 스타일링 속성에 제약을 받아 z-index 등으로 번거로운 후처리를 해줘야한다.<br />

이러한 상황에서 portal을 통해 독립적인 구조와 부모-자식 관계를 동시에 유지할 수 있다면, z-index 등 부모 컴포넌트의 제약에서 벗어날 수 있다. <br />

<h3 style="color:#0230">예시</h3>

```javascript
import React, { useEffect } from 'react'
import { auth_user } from '../_actions/actions'
import { useSelector, useDispatch } from 'react-redux'

export default function(Component, isLogin) {
  function Authentication(props) {
    let user = useSelector(state => state.user)
    //user state 사용
    const dispatch = useDispatch()

    useEffect(() => {
      dispatch(auth_user()).then(res => {
        if (!res.payload.id) {
          if (isLogin) {
            props.history.push('/login')
          }
        } else {
          if (isLogin === false) {
            props.history.push('/')
          }
        }
      })
    }, [])

    return <Component {...props} user={user} />
  }
  return Authentication
}
// user에 관한 state를 가져와서, authentication 정보를 확인한다.
// 만약 로그인이 안된 경우(state에 id가 없을 경우) 로그인이 필요한 페이지로는 갈 수 없다. 반대의 경우도 마찬가지!
```

```javascript
import Auth from './Authentication'
;<Switch>
  <Route exact path="/" component={Auth(Main, null)} />
  <Route exact path="/login" component={Auth(Login, false)} />

  <Route exact path="/upload" component={Auth(UploadForm, true)} />
</Switch>
```
