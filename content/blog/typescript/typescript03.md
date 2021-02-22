---
title: 'TypeScript Basic 3'
date: 2021-01-28 11:21:13
category: 'typescript'
draft: false
---

## 타입스크립트 BASIC 03

### Expressing function types effectively in TypeScript

단순히 인자값을 타입지정하는 것이 아닌 RETURN 값과 내부적인 요소들 또한 효과적인 타입지정이 가능하다.<BR />
자바스크립트 함수와 비교하며 또 변환해보며 얼마나 `효과적`인지 알아보자.

```tsx
// Example Javascript Function 1
function jsAdd(num1, num2) {
  return num1 + num2
}

//jsAdd typescript convert(숫자를 넘겨주고 숫자를 리턴해야함)
function add(num1: number, num2: number): number {
  return num1 + num2
}

// 복잡한 Example Javascript Function 2
function jsFetchNum(id) {
  // code...
  // code...
  // code...
  return new Promise((resolve, reject) => {
    resolve(100)
  })
}

// jsFetchNum typescript convert
// 문자열 타입의 `id` 인자를 받아 프로미스를 리턴하는데 숫자의 데이터를 반환
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
printMessage() // 'default message'

 // Rest Parameter (동일한 타입의 인자를 전달할때)
 // a,b 인자 모두 number 타입의 이며 number 타입의 RETURN 값을 반환
  function addNumbers(...numbers: number[]): number {
    return numbers.reduce((a,b)=>a+b)
    // + reduce 메서드는 배열의 각 요소에 대해 callback을 실행하며 단 1개의 출력 결과를 만듭니다.
  }

  !(error) console.log(addNumbers(1, 2, "ss"));
  console.log(addNumbers(1, 2, 3, 4)); //10
  console.log(addNumbers(1, 2)); //3
```
