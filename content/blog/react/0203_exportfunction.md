---
title: '[ Ch # 04 ] Reactively Common Function Export '
date: 2021-02-03 18:21:13
category: 'react'
draft: false
---

## React 요소가 가미된 공통함수 만들기

개발을 하며 REACT의 최대 장점 중 하나를 뽑자면, 컴포넌트화 라고 생각한다. <br />
이번엔 리액트 요소를 사용하여 공통함수를 정의하고 사용하는 과정을 해보려고한다.

```js
// commonfn/index.ts
import { useMemo } from 'react'
// render 하는 과정에서 특정 값이 바뀌었을 대만 연산을 실행하고, 원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 재사용하는
// React hooks 인 useMemo가 가미된 공통 함수를 정의해 보자!

export const mainCommonFn = (type: string) => {
  //추출할 함수 정의
  const changeBoolean: boolean = useMemo(() => {
    if (type === 'convertFalse') {
      return false
    } else if (type === 'convertTrue') {
      return true
    }
  }, [type])

  // jsx 또는 tsx의 속성을 사용하려면 return 을 통한 반환값을 줘야한다.
  // 이성질을 이용해여 공통함수의 껍데기를 정의하고 그안에서 function을 분리하여 return으로 추출하게 되면
  // react요소를 사용한 함수를 뽑아낼 수 있다.
  return { changeBoolean }
}
```

### \* 정의한 공통함수를 사용해보자 !!

```tsx
// main.tsx
import React, { useState,useEffect } from 'react'
import { mainCommonFn } from './commonfn'
// 공통함수를 import
const Main = function() {
  const [state, setState] = useState<string>('convertFalse')
  const { changeBoolean } = mainCommonFn(state)
  // import한 mainCommonFn내의 changeBoolean 공통함수를 구조 분해하여 string 타입인 state값을 전달
  // 버튼클릭시 'convertFalse'/'convertTrue'를 전달하여 작용된 연산값을 반환

  useEffect(() => {
      console.log(changeBoolean)
      // changeBoolean 값이 바뀔때 마다 changeBoolean를 콘솔로그를 찍게 해놨다.
      // 이렇게 하면 useMemo를 따로 선언하지 않고도 React useMemo 속성을 이용한 기능활용이 가능하다.
      // 물론 코드의 길이도 짧아지고 리액트스러운 개발을 할수있다고 생각한다.
  }, [changeBoolean])

  return (
      <React.Fragment>
         <button onClick={() => setState('convertTrue')}>buttonTrue</button>
         // console.log .. true
         <button onClick={() => setState('converFalse')}>buttonFalse</button>
         // console.log .. false
      <React.Fragment>

  )
}
export default Main
```
