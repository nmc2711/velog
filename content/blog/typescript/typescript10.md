---
title: 'TypeScript Object Oriented Programming Start 3'
date: 2021-02-27 18:21:13
category: 'typescript'
draft: false
---

## Absraction 을 통한 객체지향 코드 개선

```tsx
type CoffeeCup = {
  shots: number
  hasMilk: boolean
}
class CoffeeMaker {
  private BEANS_GRAMM_PER_SHOT: number = 7
  private coffeeBeans: number = 0

  private constructor(coffeeBeans: number) {
    this.coffeeBeans = coffeeBeans
  }

  static makeMachine(coffeeBeans: number): CoffeeMaker {
    return new CoffeeMaker(coffeeBeans)
  }

  fillCoffeBeans(beans: number) {
    if (beans < 0) {
      throw new Error('커피콩의 크기는 0 보다 커야한다 !')
    }
    this.coffeeBeans += beans
  }

  private grindBeans(shots: number) {
    console.log(`grinding beans for ${shots}`)
    if (this.coffeeBeans < shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT) {
      throw new Error('커피콩이 부족한데여..?')
    }
    this.coffeeBeans -= shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT
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

// 이런식으로 오브젝트를 구성하는 메서드들이 많아진다면 복잡해질수있다.
// make 자체에서 생성과 관련된 메서드들을 자체적으로 호출하고 private 화 함으로써
// 커피를 만들기위한 makeCoffee,fillCoffeBeans 접근을 유도함으로 간단하게 구현가능

const maker = CoffeeMaker.makeMachine(32)
maker.fillCoffeBeans(32)
maker.makeCoffee(2)
// 두개의 메서드로만 커피생성을 추상화할수있다.
```

### vs InterFace

```tsx
type CoffeeCup = {
  shots: number
  hasMilk: boolean
}
// interface 타입선언에서 규약을 맺는 역활
interface CoffeeMaker {
  makeCoffee(shots: number): CoffeeCup
}

interface CommercialCoffeeMaker {
  makeCoffee(shots: number): CoffeeCup
  fillCoffeBeans(beans: number): void
  clean(): void
}

class CoffeeMachine implements CoffeeMaker, CommercialCoffeeMaker {
  private BEANS_GRAMM_PER_SHOT: number = 7
  private coffeeBeans: number = 0

  private constructor(coffeeBeans: number) {
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

// 인터페이스에서 정의된 메서드만 사용가능
// const maker2: CommercialCoffeeMaker = CoffeeMachine.makeMachine(32)
// maker2.fillCoffeBeans(32)
// maker2.makeCoffee(2)
// maker2.clean()

//
class AmateurUser {
  constructor(private machine: CoffeeMaker) {}
  makeCoffee() {
    const coffee = this.machine.makeCoffee(2)
    console.log(coffee)
  }
}

class ProBarista {
  constructor(private machine: CommercialCoffeeMaker) {}
  makeCoffee() {
    const coffee = this.machine.makeCoffee(2)
    console.log(coffee)
    this.machine.fillCoffeeBeans(45)
    this.machine.clean()
  }
}
const maker: CoffeeMachine = CoffeeMachine.makeMachine(32)

const amateur = new AmateurUser(maker)
const pro = new ProBarista(maker)

// 동일한 coffee machine에서 제약된 interface 를 각각 사용가능

amateur.makeCoffee(7)
pro.makeCoffee(8)
pro.fillCoffeBeans(45)
pro.clean()
```
