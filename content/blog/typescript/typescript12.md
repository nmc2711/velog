---
title: 'TypeScript Object Oriented Programming Polymorphism'
date: 2021-03-03 18:21:13
category: 'typescript'
draft: false
---

## Polymorphism 공통적인 요소를 이용해 다양한 클래스 구현

```tsx
type CoffeeCup = {
  shots: number
  hasMilk?: boolean
  hasSugar?: boolean
}
interface CoffeeMaker {
  makeCoffee(shots: number): CoffeeCup
}

class CoffeeMachine implements CoffeeMaker {
  private BEANS_GRAMM_PER_SHOT: number = 7
  private coffeeBeans: number = 0

  constructor(coffeeBeans: number) {
    this.coffeeBeans = coffeeBeans
  }

  static makeMachine(coffeeBeans: number): CoffeeMachine {
    return new CoffeeMachine(coffeeBeans)
  }

  fillCoffeBeans(beans: number) {
    if (beans < 0) {
      throw new Error('커피콩의 크기는 0 보다 커야한다 !')
    }
    this.coffeeBeans += beans
  }

  clean() {
    console.log('커피머신 청소중...')
  }

  private grindBeans(shots: number) {
    console.log(`grinding beans for ${shots}`)
    if (this.coffeeBeans < shots * CoffeeMachine.BEANS_GRAMM_PER_SHOT) {
      throw new Error('커피콩이 부족한데여..?')
    }
    this.coffeeBeans -= shots * CoffeeMachine.BEANS_GRAMM_PER_SHOT
  }

  private preheat(): void {
    console.log('커피머신 데우는중 ..... fire...!')
  }

  private extract(shots: number): CoffeeCup {
    console.log(`커피 내리는중 ....${shots} shots...`)
    return {
      shots,
      hasMilk: false,
    }
  }
  makeCoffee(shots: number): CoffeeCup {
    this.grindBeans(shots)
    this.preheat()
    return this.extract(shots)
  }
}

class CaffeLatteMachine extends CoffeeMachine {
  private steamMilk(): void {
    console.log(`steaming some milk...`)
  }
  constructor(beans: number, public readonly serialNumber: string) {
    super(beans)
  }

  makeCoffee(shots: number): CoffeeCup {
    const coffee = super.makeCoffee
    this.steamMilk()
    return {
      ...coffee,
      hasMilk: true,
    }
  }
}
class SweetCoffeeMaker extends CoffeeMachine {
  makeCoffee(shots: number): CoffeeCup {
    const coffee = super.makeCoffee
    return {
      ...coffee,
      hasSugar: true,
    }
  }
}
const machines: CoffeeMaker[] = [
  // 동일한 interface CoffeeMaker 를 사용 즉 다른 public 요소가 있지만 이렇게 배열로
  // 묶음으로 사용자가 makeCoffee 매서드만을 이용하여 커피를 만들게 유도가능
  new CoffeeMachine(16),
  new CaffeLatteMachine(16, '시리얼넘버 abc'),
  new SweetCoffeeMaker(16),
  new CoffeeMachine(17),
  new CaffeLatteMachine(17, '시리얼넘버 dfg'),
  new SweetCoffeeMaker(17),
]
machines.forEach(machine => {
  console.log('---')
  machine.makeCoffee(1)
})
// 다양성의 특징으로 공통된 makecoffee 매서드를 통한 분화된 기능의 오브젝트 생성가능
```
