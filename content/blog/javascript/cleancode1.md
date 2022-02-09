---
title: '클린코드 자바스크립트 STEP 1'
date: 2022-02-08 16:21:13
category: 'javascript'
draft: false
---

### \* 01 - var를 지양하자

<br />
왜 그럴까? <br /><br />

<b>var</b>는 함수 스코프, <br />그와 달리 <b>let & const</b>는 블록단위 스코프와 함께 Temporal Dead Zone(TDZ)를 가지게 된다. <br />
이로인해 상대적 최신 문법인 let & const는 <b style="color:red">안전한 코드작성</b>을 할 수 있다. <br />
그렇다면 뭐가 안전하다는 걸까? <br />

```js
var name = '이름'
var name = '이름2'
var name = '이름3'

// var를 사용하여 같은 이름으로 변수를 선언하고 할당하였다. 하지만 아무런 오류가 발생하지 않는다!

let name = '황상한'
let name = '바보'

// SyntaxError Identifier 'name' has already been declared (2:4) 이미 선언했기 때문에 또 할 수 없다.
```

위와 같은 차이로 간단하게 나타낼 수 있는데, var를 사용한 변수 선언과 할당이 간단한 코딩으로는 편리할 수도 있다. <br />
하지만 프로젝트의 규모와 복잡도에 따라 코드의 양은 매우 증가할것이다. 코드를 기억못하는 상황이 올 수도 있다.<br />
따라서 엄격한 규칙에 따른 범위를 가진 let & const를 활용하는 것이다. <br /><br />

그렇다면 <b>let & const</b>의 차이는 뭘까? <br /><br />
핵심은 <b style="color:red">재할당</b>이다.

```js
let animal

animal = '호랑이'

animal = '사자'

// let 먼저선언하고 언제든지 재할당이 가능하다.
```

```js
const animal

animal = '뱀'

// SyntaxError Const declarations require an initialization value

// const는 선언과 할당이 동시에 일어나야 한다.
```

<br /><br />

### \* 02 - functional scope & block scope

"전역변수를 많이 사용하지 말자" 라는 이야기를 들어본적이 있을 것이다.

```js
var global = '전역'

if (global === '전역') {
  var global = '지역'

  console.log(global) // 지역
}

console.log(global) // 지역

// 전역적으로 선언한 변수에 if문 내의 가두어 선언하여 할당한 변수가 영향(오염)을 주었다.
```

왜 그럴까? <br /><br />
var는 함수단위 스코프이기 때문이다. if문은 함수가 아니다. <br />
따라서 block 단위 스코프로 바뀌지않는 이상 불안정한 영향이 계속 될수 밖에 없다.

```js
let global = '전역'

if (global === '전역') {
  let global = '지역'

  console.log(global) // 지역

  // 해당 중괄호안에서 let은 블록단위로 지역변수로서의 역활을 잘해주고있다.
}

console.log(global) // 전역

// 더불어 전역공간에서는 전역변수로서의 역활 또한 잘하고있다. 굉장히 안전해짐
```

<br />
추가로 let대신 const를 사용하자라는 이야기도 들어본적이 있을것이다. <br />
왜 그럴까? <br />

```js
const person = {
  name: '상한',
  age: '28',
}

person = {
  name: '오징어',
  age: '28',
}

// SyntaxError 재할당 불가
// 그렇다면 어떻게 값을 변경할 수 있는데?

person.name = '오징어'

console.log(person) // { name: '오징어', age: 28 }
```

그러면 왜 let 보다 const를 우선적으로 사용하라는 걸까?<br />
변수를 선언하는 시점에는 재할당이 필요할지 잘 모르는 경우가 많다.(핵심)<br />
그리고 객체는 의외로 재할당을 하는 경우가 드물다.<br />
따라서 변수를 선언할 때에는 일단 const 키워드를 사용하도록 하자.<br />
반드시 재할당이 필요하다면(반드시 재할당이 필요한지 한번 생각해 볼 일이다.)<br />
그때 const를 let 키워드로 변경해도 결코 늦지 않는다.<br /><br />

### \* 03 - 전역 공간 사용 최소화

<br />
전역 공간이 무엇일까?<br />

전역공간은 말그대로 (최상위 공간)이다. - 브러우저 환경에서는 <b>window</b>, Node 환경에서는 <b>global</b>라고 할 수 있다.

```js
// global.html

...

<body>
  <script src="./index.js" />
  <script src="./index-1.js" />
</body>

...

// index.js
var globalVar = 'global';

console.log(globalVar) // global


// index-1.js

console.log(window.globalVar) // global
console.log(globalVar) // global
```

<br />
index.js에서 var를 통하여 선언한 변수가 index-1.js 에서도 접근이 가능하고, 몽키패치로 인해 <br />
window.~로도 접근이 오류없이 접근이 가능하다. <br />
파일을 나눈다고 할지라도 scope자체가 나뉘어 지지 않고 공유된다는 의미이다. <br /><br />

그렇다면 어떻게 주의 하라는 걸까? <br />

1. 전역 변수사용을 지양해야 할것이다.
2. 지역 변수사용을 하도록 한다.
3. winodw, global에대한 직접적인 조작을 지양한다.
4. IIFE(즉시실행함수), Module, Closure를 통하여 명확한 스코프를 나눈다.
