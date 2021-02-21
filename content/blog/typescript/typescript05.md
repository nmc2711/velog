---
title: '[ Ch # 04 ] TypeScript Basic Type'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

```tsx
//!! vs discriminated union type으로 차이점 판단해보기
type SuccessState = {
  result: 'success'
  response: {
    body: string
  }
}
type FailState = {
  result: 'fail'
  reason: string
}
type LoginState = SuccessState | FailState
// 성공 실패중 성공만 정의
function login(): LoginState {
  return {
    result: 'success',
    // 키값이 일치하는 타입의 값을 반환
    response: {
      body: 'login',
    },
  }
}
//example: printLoginState(state)
function printLoginState(state: LoginState): void {
  // obj key로 판단
  if (state.result === 'success') {
    console.log(`success ! ${state.response.body}`)
  } else {
    console.log(`fail ㅠ ${state.reason}`)
  }
}
// 즉 같이 사용하는 키값을 분기로 지정하여 유연하게 타입 판단.
-----------------------------------------------------------
// Intersection Type : & 다양한 타입을 하나로 묶어서 사용

type Student = {
  name: string
  score: number
}
type Worker = {
  empolyeeId: number
  work: () => void
}

function interWork(person: Student & Worker) {
  console.log(person.name, person.empolyeeId, person.work)
}
interWork({
  name: 'han',
  score: 100,
  empolyeeId: 12,
  work: () => {},
})

// enum :여러가지 관련된 상수값들을 모아 정의
// in javascript
const MAX_NUM = 6;
const MONDAY =0;
const TUESDAY= 1;
const DAYS_ENUM =Object.freeze({
    "MONDAY":0,"TUESDAY":1,
})
const dayOfToday = DAYS_ENUM.MONDAY

// in typescript
enum Days {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Satarday,
    Sunday
}
console.log(Days.Tuesday) // 1
const day = Days.Satarday
console.log(day) // 5

enum DaysV2 {
    Monday = 1, // 시작 값 변경가능
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Satarday,
    Sunday
}
enum DaysV3 {
    Monday = 'mon', // 문자열 할당가능
    Tuesday= 'the',
    Wednesday= 'wed',
    Thursday= 'thu,
    Friday= 'fri',
    Satarday= 'sat',
    Sunday= 'sun'
}
console.log(DaysV2.Tuesday) // 2
console.log(DaysV2.Sunday) // sun


//but ..enum에 다시 값을 할당할떄 자유롭게 할당가능 하므로 비추..
ex)  let day:DaysV3 = DaysV3.Friday
day = 10 이 가능하다...

// 대체할 보통의 union type선언
type DaysCommon = 'Monday' | 'Tuesday' |'Wednesday';
let dayOfWeek :DaysCommon ='Monday'
(x) dayOfWeek = 'sanghan'
dayOfWeek = 'Tuesday'
```
