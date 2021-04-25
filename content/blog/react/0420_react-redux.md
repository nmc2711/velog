---
title: '[ Ch # 14 ] 리덕스 in REACT !'
date: 2021-04-20 18:21:13
category: 'react'
draft: false
---

## 리액트에서 리덕스 구조 설계 및 적용

개인적으로 context api를 선호하긴하지만 redux를 통해 pr을 해보며 상태관리를 해보려한다.<br />

모든 상태관리 라이브러리가 그렇지만 ? <br />
단방향성 데이터 흐름 유도.. 데이터가 한방향으로만 흐르게 유도하기 때문에 데이터 흐름을 예측하기 쉽다.<br /><br />

state를 관리하는 store에서 상태를 관리하고 컴포넌트는 변경된 상태를 보여주기만 하는 용도로 쓰인다.<br />

#### BASIC FLOW

- 4가지의 플로우를 기억하면 상태관리를 하면된다.

// action <br />

action ? 무슨일이 일어났는지 설명해주는 `객체`이다.<br />
=> 행위를 지정해주고 행동을 반환한다고 할 수 있다.<br />

// reducer <br />

reducer ? state와 action을 파라미터로 받아와서 state를 변경하는 함수이다.<br />
=> action의 지정명치에 따른 연산을하고 상태를 변경,유지한다.<br />

// store <br />

store ? 상태값의 데이터가 저장되는 공간. <br />
=> 결국 provider 를 통해 전달되고 변경되는 상태가 app 전역에 전달되는 흐름창고. <br />

// dispatch <br />
dispatch ? action 발생 <br />

---

pr 예시:
https://github.com/nmc2711/redux-react-vo2
