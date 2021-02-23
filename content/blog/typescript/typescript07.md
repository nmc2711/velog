---
title: 'TypeScript Simple Project '
date: 2021-02-24 18:21:13
category: 'typescript'
draft: false
---

## Simple Project # 1

### 계산함수 구현하기

```tsx
type Command = 'add' | 'substract' | 'multiply' | 'divide' | 'remainder'
// union type 정의

function calculate(command: Command, a: number, b: number): number {
  switch (Command) {
    case 'add':
      return a + b
    case 'substract':
      return a - b
    case 'multiply':
      return a * b
    case 'divide':
      return a / b
    case 'remainder':
      return a % b
    default:
      throw Error('unknown command')
  }
}

console.log(calculate('divide', 4, 2)) // 2
```

### 좌표게임 구현하기

```tsx
const position = { x: 0, y: 0 }

function move(direction: 'up' | 'down' | 'left' | 'right') {
  switch (direction) {
    case 'up':
      return position.y + 1
      break
    case 'down':
      return position.y - 1
      break
    case 'left':
      return position.x - 1
      break
    case 'right':
      return position.x + 1
      break
    default:
      throw Error(`unknown direction : ${direction} !`)
  }
}

console.log(move('up')) // {x:0,y:1}
```

### API NETWORK CALL RESULT 구현하기

```tsx
type LoadingState = {
  state: 'loading'
}
type SuccessState = {
  state: 'success'
  respone: {
    body: string
  }
}
type FailState = {
  state: 'fail'
  reason: string
}

type ResourceLoadState = LoadingState | SuccessState | FailState

function printLoginState(callState: ResourceLoadState) {
  const { state } = callState
  switch (state) {
    case 'loading':
      console.log('로딩중입니다..ㅠ')
      break
    case 'success':
      console.log(`${callState.respone.body}..^^`)
      break
    case 'loading':
      console.log(`${callState.reason}..oops !!!!`)
      break
    default:
      throw new Error(`unknown State : ${state}`)
  }
}
printLoginState({ state: 'success', response: { body: 'mainListCall' } }) // 'mainListCall'
```
