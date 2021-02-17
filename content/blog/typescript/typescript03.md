---
title: '[ Ch # 03 ] TypeScript Basic Type'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

## 자바스크립트 함수를 변환해보기

```tsx
// javascript fn1
function jsAdd(num1, num2) {
  return num1 + num2
}
// fn1 typescript convert(숫자를 넘겨주고 숫자를 리턴해야함)
function add(num1: number, num2: number): number {
  return num1 + num2
}

// javascript fn2
function jsFetchNum(id) {
  // code...
  // code...
  // code...
  return new Promise((resolve, reject) => {
    resolve(100)
  })
}
// fn2 typescript convert (string을 받아 프로미스를 리턴하는데 숫자의 데이터를 리턴)
function fetchNum(id: string): Promise<number> {
  // code...
  // code...
  // code...
  return new Promise((resolve, reject) => {
    resolve(100)
  })
}

// Default Parameter (아무런 값을 전달하지않아도 기본 타입과 파라미터를 전달가능)
function printMessage(message: string = 'default message') {
  console.log(message)
}
printMessage()

 // Rest Parameter (동일한 타입의 인자를 전달할때)
  function addNumbers(...numbers: number[]): number {
    return numbers.reduce((a,b)=>a+b)
  }

  (x)console.log(addNumbers(1, 2, "ss")); //error
  console.log(addNumbers(1, 2, 3, 4)); //10
  console.log(addNumbers(1, 2)); //3
```
