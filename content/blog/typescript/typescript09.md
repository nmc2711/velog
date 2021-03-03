---
title: 'TypeScript Object Oriented Programming Start 2'
date: 2021-02-05 18:21:13
category: 'typescript'
draft: false
---

## Class TypeScript Module Encapsulation & invalid

```tsx
// 캡슐화 및 invalid 코드 보완
type CoffeeCup = {
  shots: number
  hasMilk: boolean
}
// public
// private  외부에서 접근 불가
// protected 객체를 상속 받아야만 접근가능

class CoffeeMaker {
  private BEANS_GRAMM_PER_SHOT: number = 7
  private coffeeBeans: number = 0

  // 외부에서 접근하지 않는 코드를 private

   private constructor(coffeeBeans:number) {
      this.coffeeBeans = coffeeBeans
   }

   // constructor private화 함으로 다른 함수를 통한 생성자 접근을 유도할 수 있다.

  static makeMachine(coffeeBeans:number):CoffeeMaker {
      return new CoffeeMaker(coffeeBeans)
  }
  fillCoffeBeans(beans:number) {
      if (beans < 0) {
          throw new Error('커피콩의 크기는 0 보다 커야한다 !')
      }
      this.coffeeBeans += beans;
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
}
const maker = new CoffeeMaker(32)
maker.fillCoffeBeans(32)
```
