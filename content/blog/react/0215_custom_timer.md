---
title: '[ Ch # 08 ] 타이머 구현 하기'
date: 2021-02-15 18:21:13
category: 'react'
draft: false
---

한번 만들어 놓으면 유용하게 쓰일 타이머 기능

```tsx
// timer.tsx
import React, { useState, useRef } from 'react'

export default (initialStartValue = 0, initialEndValue = 30) => {
  const [time, setTime] = useState<number>(initialStartValue)
  // 초 state 세팅(초기값과 끝값을 인자로 받는다.)
  const [running, setRunning] = useState<boolean>(false)
  // 타이머의 stop/start 판단 state

  const timerRef = useRef<any>()
  // ref를 통한 api 접근

  const startTimeout = () => {
    !running && setRunning(true)
    // 타이머의 시작과 멈춤 꼬임일 방지하기위한 Flag Stop 상태 일때만 시작 함수 실행

    if (running === true) {
      // 혹여 start 상황일경우
      return null
    }

    timerRef.current = setInterval(() => {
      setTime(prevState => {
        if (prevState === initialEndValue) {
          // 지정한 종료시간이 되면 타이머기능 멈춤
          stopTimeout()
          // 종료와 동시 현재 state 반환
          return prevState
        } else {
          return prevState + 1
        }
      })
    }, 1000)
  }

  const stopTimeout = () => {
    clearInterval(timerRef.current)
    setRunning(false)
  }
  // 멈춤기능

  const clearTimer = () => {
    stopTimeout()
    setTime(initialStartValue)
  }
  // 초기화 기능

  return { startTimeout, stopTimeout, time, clearTimer }
}
```

```tsx
// index.tsx
import React from 'react'
import useTimer from './timer'

const Main = function() {
  const { time, startTimeout, stopTimeout, clearTimer } = useTimer(0, 35)
  // theme 상태값과 toggle 함수 객체분해
  return (
    <div>
      <h1>리액트 타이머</h1>
      <h3>{time}</h3>
      <button onClick={startTimeout}>시작</button>
      <button onClick={stopTimeout}>멈춤</button>
      <button onClick={clearTimer}>초기화</button>
    </div>
  )
}
```
