---
title: '[ Ch # 04 ] TypeScript Basic Type'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

```tsx
//Array
let fruits: string[] = ['tomato', 'banna']
let scores: Array<number> = [1, 2, 3]
function printArray(fruits: readonly string[]) {
  // readonly 를 쓰게 되면
  // (x) fruits.push('orange') 배열을 변경할수없다.
  // 두 타입선언의 차이점이라고 하면 Array<number>는 readonly를 사용할수없다.
}

//Tuple ->interface,type alias,class 대체 해서 보통사용
let student: [string, number]
student = ['name', 1231]


  // type Alias 새로운 타입을 정의할수있다.
  type Text = string;
  const names: Text = "han";

  type Num = number;
  type Student = {
    name: string;
    age: number;
  };
  const students: Student = {
    name: "han",
    age: 90,
  };

 // string Literal Types
  type Name = "name";
  let hanName: Name;

  (x) hanName = "nono"; // 'name'이라는 문자열만 할당가능
  type JSON = 'json'
  const jsonType:JSON ='json'

  // union Types : or 발생할수있는 다양한 케이스중 하나만 정의하고싶을때
  type Direction ='left' |'right'|'up'|'down'
  function move (direction:Direction) {
    console.log(direction)
  }
  move('down')

  type TileSize = 8|16|27;
  const tile:TileSize = 16;

  // example function:login => success fail
  type SuccessState = {
    response: {
      body:string
    }
  }
  type FailState = {
    reason:string
  }
  type LoginState = SuccessState | FailState
  // 성공 실패중 성공만 정의
  function login (): LoginState {
    return {
      response: {
        body:'login'
      }
    }
  }
  //example: printLoginState(state)
  function printLoginState(state:LoginState):void {
    // obj key로 판단
      if('response' in state) {
        console.log(`success ! ${state.response.body}`)
      }else {
        console.log(`fail ㅠ ${state.reason}`)
      }
    }

```
