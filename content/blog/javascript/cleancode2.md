---
title: '클린코드 자바스크립트 STEP 2'
date: 2022-02-10 16:21:13
category: 'javascript'
draft: false
---

### \* 04 - 임시 변수 제거하기

<br />
임시 변수? <br /><br />
자바스크립트를 사용하여 개발할 경우, 기존의 Scope 를 다룰때, <br />
전역공간 사용을 지양 하는걸 목표 한다.  <br />
<b>임시변수</b>란, 특정 Scope 안에서 전역변수처럼 활용되는 변수이다. <br />

```js
function getObject() {
  const result = {} // 비어있는 임시 객체

  result.title = 'yes, react'
  result.text = '안녕하세요'

  return result

  // 어떻게 보면 비이있는 임시 객체로 선언된 result는 블록단위의 const이기 때문에 문제 없지 않을까라고 생각할 수 있다.
  // 하지만 getObject 함수가 비대해지고 여러 곳에서 사용된다면 전역공간과 다름없는 상황이 발생할 수 있다.
}
```

그렇다면 어떻게 예방할 수 있을까? <br />
해답은 불필요한 범위의 변수선언을 금하여 임시변수에 대한 접근을 막고 <br />
명료한 반환을 해주는 것이다. <br />

```js
function getObject() {
  // ...
  return {
    title: 'yes, react',
    text: '안녕하세요',
  }

  // ...
  const result = {
    title: 'yes, react',
    text: '안녕하세요',
  }
  return result
  // ...
}
```

습관처럼 사용되면 큰규모의 개발에서 임시변수 사용은 <b style="color:red">치명적</b>이다. <br />
임시변수는 대부분 접근과 명령에 대한 로직이 많기 때문에 디버깅이 어렵고, 뭉치가 많기때문에 유지보수 또한 힘들어 진다.<br />
기능에 맞는 함수의 세분화, 명료한 반환, map,filter,reduce .. 와 같은 고차 함수를 사용하며 명령형이 아닌 선언형 코드 스타일의 <br />
전환으로 임시변수의 습관적 사용을 제한할 수 있다. <br /><br />

### \* 05 - 호이스팅 주의하기

<br />
호의스팅? <br /><br />
간단하게, 런타임시기에 선언과 할당이 분리된 것을 뜻한다. (작성자가 기대하는 스코프범위를 벗어난다.)<br />
직관적인 예를 들면 var를 사용한 변수가 초기화가 제대로 되어 있지 않았을때, undefined로 최상단에 끌어올려줄 수 있는것<br />
(예상치 못하게 선언부만 최상단으로 옮겨지는 현상)

```js
var global = 0
function outer() {
  console.log(global) // undefined 호이스팅(메모리공간을 선언전에 할당)

  function inner() {
    var global = 10
    console.log(global) // 10
  }

  inner()

  global = 1

  console.log(global) // 1
}

outer()
```

```js
var sum

console.log(sum()) // 3
console.log(typeof sum) // function..? 함수도 호이스팅 된다.

function sum() {
  return 1 + 2
}
```

간단한 해결방법으로는 함수스코프대신 블록스코프를 쓰거나 선언과 동시에 할당을 마무리 해줘야 한다.(<b>const</b> 사용) <br />

```js
var sum = 10

console.log(sum) // 이전과 다르게 6이 아니고 10

function sum() {
  return 1 + 2 + 3
}
```

<br />

### \* 06 - 타입 검사

<br />
<h3>1. typeof</h3>

typeof 연산자는 검사대상의 타입을 문자열로 반환한다.<br />
자바스크립트의 타입정의는 크게 Primitive(원시값), Reference(참조값) 분리할 수 있는데,<br />
동적인 타입을 갖는 Reference는 기대하는 타입을 확인할 수 없는 경우가 발생한다.<br />

```js
typeof '자바스크립트' // 'string'
typeof 9999 // 'number'
typeof undefined // 'undefined'
typeof Symbol() // 'symbol'

// 기대하는 타입을 확인할 수 없는 예로
// 1. Wrapper 객체로된 원시값 / class / null
const title = new String('스파이더맨')
typeof title // 'object' ??

function myFunction() {}
class MyClass {}

typeof myFunction // 'function'
typeof MyClass // 'function' ??
typeof null // 'object' ??
```

<br />
<h3>2. instanceof</h3>

instanceof 연산자는 객체의 프로토타입 체인을 검사하는 연산자이다.
검사하는 대상의 참조값을 어떤 객체의 인스턴스인지 여부를 판단하여 반환한다.

```js
const arr = []
const myFunc = function() {}
const date = new Date()

arr instanceof Array // true
myFunc instanceof Function // true
date instanceof Date // true

arr instanceof Object // true ??
myFunc instanceof Object // true ??
date instanceof Object // true ??
```

사실 모든 인스턴스는 자바스크립트 특성상 본질적으로 객체의 인스턴스이다. <br />
함수, 배열, 데이트 객체 같은경우 프로토타입 체인을 타기 때문에 결국 최상위의 Object를 가르킨다.<br />

<h3>3. Object.prototype.toString</h3>

Object.prototype.toString는 객체를 나타내는 문자열을 반환한다.<br />
여기에 .call 메소드를 사용하면 오버라이드된 모든 타입의 값의 타입을 알아낼 수 있다.<br />

```js
function myFunction() {}
const date = new Date()
class MyClass {}

Object.prototype.toString.call(new String('javascript')) // [object String]
Object.prototype.toString.call(myFunction()) // [object Function]
Object.prototype.toString.call(date) // [object Date]
Object.prototype.toString.call(undefined)) // [object Undefined]
Object.prototype.toString.call(null)) // [object Null]
Object.prototype.toString.call(MyClass)) // [object Function] ??
```
