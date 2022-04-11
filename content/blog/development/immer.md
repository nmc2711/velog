---
title: 'immer'
date: 2022-03-15 16:21:13
category: 'development'
draft: false
---

## immmer.js ?

react에서 불변성을 유지하는 코드를 작성하기 쉽게 해주는 라이브러리입니다.<br />

### 불변성이란?

쉽게 말하면 상태를 변경하지 않는 것이.<br />
상태를 변경하는데, 상태를 변경하지 않으면서 원하는 상태를 바꾼다는게 어불성설이지만 react가 어떻게 컴포넌트를 유지하는지(기본 속성) <br />알면 왜 저런 말이 나오는지 이해하게 된다.

react는 기본적으로 부모 컴포넌트가 리렌더링을 하면 자식 컴포넌트도 리렌더링하게 된다.<br />
즉, 얕은 비교를 통해 새로운 값인지 아닌지를 판단한 후 새로운 값인 경우 리렌더링을 하게 된다<br />
여기서 얕은 비교란 간단히 말하자면 객체, 배열, 함수와 같은 참조 타입들을 실제 내부 값까지 비교하지 않고 동일 참조인지<br />
(동일한 메모리 값을 사용하는지)를 비교하는 것을 뜻한다.<br />

#### 좀 더 이해가 쉽게!

우리가 컴포넌트를 리렌더링 해야하는 상황이 있다고 가정하고, 타입이 배열인 state를 바꿀 것 이다.<br />
이때, state.push(1)을 통해 state 배열에 직접 접근하여 요소를 추가한다.<br />
우리는 push 전과 다른 값이라고 생각하지만, <br />
리엑트는 state라는 값은 새로운 참조값이 아니기 때문에 이전과 같은 값이라고 인식하고 리렌더링 하지 않는다.<br />
즉, 위 이유로 우리가 state를 바꾸고 돔을 다시 만들려면, 새로운 객체 or 배열을 만들어 새로운 참조값을 만들고, <br />
react에게 이 값은 이전과 다른 참조값임을 알려야하는 것이다.<br />

```javascript
import produce from 'immer'

const baseState = [
  {
    todo: 'Learn typescript',
    done: true,
  },
  {
    todo: 'Try immer',
    done: false,
  },
]

const nextState = produce(baseState, draftState => {
  draftState.push({ todo: 'Tweet about it' })
  draftState[1].done = true
})
```

immer에서 우리가 쓸 함수는 오직 produce만 알면 된다.<br />
2가지의 파람을 가져오고 첫번째는 수정하고 싶은 객체/배열, 두번째는 첫번째 파라미터에 할당된 객체/배열을 바꾸는 함수이다.<br />