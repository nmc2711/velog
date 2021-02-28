---
title: 'TypeScript Object Oriented Programming Start'
date: 2021-02-05 18:21:13
category: 'typescript'
draft: false
---

## TypeScript 객체지향 프로그래밍 OOP STEP 1

### OOP Concept ?

프로그램을 객체로 정의해서 객체들끼리 서로 의사소통 하도록 디자인하고 코딩해 나가는 것 <BR />
서로 관련 있는 데이터와 함수를 여라가지 오브젝트로 정의해서 프로그래밍 하는 것<BR />
모든 것이 얽혀있고 수직적인 절차지행적 프로그래밍과 달리 OOP는 문제가 있는곳만 처리해서 해결,재생산,수정(유지보수) 가능

### OOP 4가지원칙

1. Encapsulation 캡슐화가 되어 있어야 한다.
2. Abstraction 추상성이 좋아야 한다.
3. Inheritance 상속을 이용해서 코드의 재사용성 증가
4. Polymorphism 다향한 형태의 복제

### 절차지행적 코드와 비교하여 알아보기

```tsx
// 절차지행적 커피내리는 로직
type CoffeeCup = {
  shots: number
  hasMilk: boolean
}

const BEANS_GRAMM_PER_SHOT: number = 7
let coffeeBeans: number = 0

function makeCoffee(shots: number): CoffeeCup {
  if (coffeeBeans < shots * BEANS_GRAMM_PER_SHOT) {
    throw new Error('커피콩이 부족한데여..?')
  }
  coffeeBeans -= shots * BEANS_GRAMM_PER_SHOT

  return {
      shots,
      false
  }
}
coffeeBeans += 3 * BEANS_GRAMM_PER_SHOT;
const coffee = makeCoffee(2)
console.log(coffee) // {shots:2,hasMilk:false}
```

```tsx
// 객체지향적 코드로 바꿔보자..
type CoffeeCup = {
  shots: number
  hasMilk: boolean
}
class CoffeeMaker {
  static BEANS_GRAMM_PER_SHOT: number = 7
  // static 을 사용해 class레벨로 바꿔 오브젝트 생성시마다 생성하는 메모리 낭비 방지
  coffeeBeans: number = 0 // instance (object) level

  constructor(coffeeBeans:number) {
      this.coffeeBeans = coffeeBeans
      //오브젝트 생성시 초기설정
  }
 makeCoffee(shots: number): CoffeeCup {
  if (this.coffeeBeans < shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT) {
    throw new Error('커피콩이 부족한데여..?')
  }
  this.coffeeBeans -= shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT

  return {
      shots,
      false
  }
}
     // static속성은 함수에도 활용가능하다. 생성자를 안거치고 실행가능
  static makeMachine(coffeeBeans:number):CoffeeMaker {
      return new CoffeeMaker(coffeeBeans)
  }
}
const maker = new CoffeeMaker(32)
// static을 추가하기전 ..{BEANS_GRAMM_PER_SHOT:7,coffeeBeans:32}
const staticMaker = new CoffeeMaker(12)
// static을 추가 ..{coffeeBeans:12}
const maker2 = CoffeeMaker.makeMachine(3) // ..{coffeeBeans:3}
```
