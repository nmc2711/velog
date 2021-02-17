---
title: '[ Ch # 02 ] TypeScript Basic Type'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

## 기본타입 지정해보기

```tsx
  /**
   * Javascript Type
   * Primitive :number, string, boolean, bigint, symbol,null,undefined
   * Object:function, array ...
   */

  // number
  const num: number = 1;
  const num2: number = -6;

  // string
  const str: string = "hello";

  // boolean
  const boal: boolean = true;

  // undefined (값이 있는지 없는지 아무것도 결정되지 않은 상태 )
  let name: undefined;
  (x) name = "hello";
  // => Type '"hello"' is not assignable to type 'undefined'.ts(2322)
  //  따라서 보통 undefined 에 타입을 줄경우 ?
  let age: number | undefined;
  age = 1;
  function find():number | undefined {
      return undefined
  }

  // null (텅비어있다 명확)
  (x) let person: null;
  // 이렇게 하게되면 null 역시도 할당가능하므로 보통의 경우 ?
  let person2 : string | null

   // unknown (비추... 어떠한 타입도 담을수있기 때문 보통리턴값의 타입을 모를때 사용)
  let notSure :unknown = 0;
  notSure ='he'
  notSure= true

  // any (비추... 모든것을 담을수 있다는 타입)
  let anything : any =0;
  anything ="hello";

 // void (함수에서 아무것도 리턴하지 않는다는)
 function print():void {
     console.log('hello')
     return
 }
 // never (절대 리턴값이 없거나,에러,while 의 true 와 같이 무한루프를 돌때)
 function throwError(message:string):never {
     throw new Error(message)
    // while (true) {
    // }
 }

 //object (비추...primitive타입을 제외한 모든 object을 전달할수있다.)
 let obj:object ={name:'david'}
 function acceptSomeObject(obj:object) {

 }
 acceptSomeObject({name:'sanghan'})
 acceptSomeObject([1,2,3,4])
```
