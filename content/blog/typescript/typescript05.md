---
title: 'TypeScript Basic 5'
date: 2021-01-30 18:21:13
category: 'typescript'
draft: false
---

## 타입스크립트 BASIC 05

### Discriminated Union type , Intersection Type , Enum

<BR />

**1. Discriminated Union type**

- 일반적인 문자열 리터럴 프로퍼티가 있는 타입이 `공통된 키 값`으로 존재 하여 구분하는 타입

```tsx
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
    // result 키값이 일치하는 타입의 값을 반환
    response: {
      body: 'login',
    },
  }
}
// Example: printLoginState(state)
function printLoginState(state: LoginState): void {
  // obj key로 판단
  if (state.result === 'success') {
    console.log(`success ! ${state.response.body}`)
  } else {
    // FailState 구분값 없이 인지
    // 즉 , 같이 사용하는 키값을 분기로 지정하여 유연하게 타입 판단.
    console.log(`fail ㅠ ${state.reason}`)
  }
}
```

<BR />

**2. Intersection type**

- `&` 여러타입을 합쳐서 사용하는 타입 (하나로 묶어서 ?)

```tsx
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
```

<BR />

**3. Enum**

- Enum 은 코드의 추상화를 도와주는 도구. 리터럴 타입과 유니온을 이용해 code 변수에 대한 타입의 범위를 좁혀줄 수 있다.
- 여러가지 관련된 상수값들을 모아 정의

```tsx
// In javascript
const MAX_NUM = 6;
const MONDAY =0;
const TUESDAY= 1;

const DAYS_ENUM = Object.freeze({
"MONDAY":0 , "TUESDAY":1,
// + Object.freeze() 메서드는 객체를 동결합니다. 동결된 객체는 더 이상 변경될 수 없습니다.
})

const dayOfToday = DAYS_ENUM.MONDAY

// In typescript
enum Days = {
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
console.log(DaysV3.Sunday) // 'sun'

//but ..enum에 다시 값을 할당할떄 자유롭게 할당가능 하므로 비추..
ex) let day:DaysV3 = DaysV2.Friday
day = 10 이 가능하다...

// 대체할 보통의 Union Type  구현
type DaysCommon = 'Monday' | 'Tuesday' |'Wednesday';
let dayOfWeek :DaysCommon ='Monday'
(x) dayOfWeek = 'sanghan'
dayOfWeek = 'Tuesday'

```
