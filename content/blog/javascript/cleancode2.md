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
전환으로 임시변수의 습관적 사용을 제한할 수 있다. <br />
