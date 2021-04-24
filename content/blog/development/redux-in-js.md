---
title: 'Redux in Js'
date: 2021-04-05 16:21:13
category: 'development'
draft: false
---

# Redux In javascript ?

redux는 전역적인 state 관리해주는 라이브러리.<br />
(컴포넌트가 복잡해 지고 depth가 깊어질경우 부모에서 상태관리 보다 효율적임을 추구)<br />

1. redux 구성요소<br />

   - reducer : 2개의 인자를 전달받는 , js 함수. <br />
     첫번쨰 인자는, 현재의 state 두번째 인자는,action <br />
     `reducer는 action의 type에 따라 이전의 state에 적절한 처리를 해 새로운 state를 반환합니다.`<br /><br />

   - action : redux의 state가 어떻게 변할지 알려주는 인자. <br />
     (redux에서는 state를 변경할 수 있는 유일한 방법을 store에 action 신호를 보내는 것이라고 규정)<br />
     즉, action은 app에서 store에 보내는 일종의 데이터이다.<br /><br />

2. redux store 메서드<br />

   - getState : 현재 app의 state에 접근할 때 사용<br />

   - dispatch : action을 dispatch할때 사용<br />

   - subscribe : state 변화를 들을때 사용<br />

pr 예시:
https://github.com/nmc2711/redux-in-js
