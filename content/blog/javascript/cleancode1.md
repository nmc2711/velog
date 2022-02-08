---
title: '클린코드 자바스크립트 1 DAY'
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
