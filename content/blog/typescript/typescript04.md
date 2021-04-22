---
title: 'TypeScript Basic 4'
date: 2021-01-29 18:21:13
category: 'typescript'
draft: false
---

## 타입스크립트 BASIC 04

### What is ? Array , Tuple , Type Alias, String Literal Types , Union Types

<BR />

**1. Array**

- 두가지 선언방식에서 차이점 ? `readonly` 속성으로 알아 볼 수 있다.

```tsx
let fruits: string[] = ['tomato', 'banna']
let scores: Array<number> = [1, 2, 3]

function printArray(fruits: readonly string[]) {
  // (x) fruits.push('orange') 배열을 변경할수없다.
}
```

두 타입선언의 차이점이라고 하면 Array<number>는 readonly를 사용할수없다.

즉, : type[] 과 같은 타입지정은 readonly를 통해 오브젝트의 고유성을 유지할 수있다.
<BR />
<BR />

**2. Tuple**

- 원소의 수와 `각 원소의 타입이 정확히 지정된`배열의 타입을 정의할 수 있다.
  <BR />보통 Interface, Type alias , Class로 대체 해서 보통사용

```tsx
let student: [string, number]
student = ['name', 1231]
```

<BR />

**3. Type Alias**

- 새로운 타입을 정의하며 타입에 직접 이름을 부여할 수 있다.

```tsx
type Text = string
const names: Text = 'han'

type Num = number

type Student = {
  name: string
  age: number
}

const students: Student = {
  name: 'han',
  age: 90,
}
```

<BR />

**4. String Literal Types**

- 문자열을 이용한 커스텀 형식의 엄격한 타입선언.

```tsx

type Name = "name";
let hanName: Name;
(x) hanName = "nono"; // 'name'이라는 문자열만 할당가능

type JSON = 'json'
const jsonType:JSON ='json'
```

<BR />

**4. Union Types**

- OR(또는) 의 의미 , 다양한 케이스중 하나만 정의하고싶을때

```tsx
type Direction = 'left' | 'right' | 'up' | 'down'

function move(direction: Direction) {
  console.log(direction)
}
move('down')

type TileSize = 8 | 16 | 27
const tile: TileSize = 16

// Example function:login => success fail
type SuccessState = {
  response: {
    body: string
  }
}
type FailState = {
  reason: string
}
type LoginState = SuccessState | FailState
// 성공 실패 중 `성공`만 정의
function login(): LoginState {
  return {
    response: {
      body: 'login',
    },
  }
}
// Example2: printLoginState(state)
function printLoginState(state: LoginState): void {
  // obj key로 판단하여 성공 실패를 TS가 구별하여 error 발생하지 않음.
  if ('response' in state) {
    console.log(`success ! ${state.response.body}`)
  } else {
    console.log(`fail ㅠ ${state.reason}`)
  }
}
```
