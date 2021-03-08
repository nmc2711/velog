---
title: 'TypeScript Object Oriented Programming Composition'
date: 2021-03-04 18:21:13
category: 'typescript'
draft: false
---

## Favor Composition over inheritance 상속대신에 컴포지션을 선호하라!

/\* 지나친 상속은 객체지향 프로그래밍의 목적을 해친다
기능별로 class를 구현하여 가져다 쓰는 구성으로 바꿔보자

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
// 커피 거품기
class CheapMilkSteamer {
  private steamMilk(): void {
    console.log(`steaming some milk...`)
  }
  makeMilk(cup: CoffeeCup): CoffeeCup {
    this.steamMilk()
    return {
      ...cup,
      hasMilk: true,
    }
  }
}
// 설탕 제조기
class AutomaticeSugarMixer {
  private getSugar(): void {
    console.log(`getting some sugar form jar...`)
    return true
  }
  addSugar(cup: CoffeeCup): CoffeeCup {
    const sugar = this.getSugar()
    return {
      ...cup,
      hasSugar: sugar,
    }
  }
}
class CaffeLatteMachine extends CoffeeMachine {
  constructor(
    beans: number,
    public readonly serialNumber: string,
    private milkFrother: CheapMilkSteamer
  ) {
    //구성된 class CheapMilkSteamer로  부터 받아와서 사용하여 상속의 문제점 해결
    super(beans)
  }

  makeCoffee(shots: number): CoffeeCup {
    const coffee = super.makeCoffee
    return this.milkFrother.makeMilk(coffee)
  }
}
class SweetCoffeeMaker extends CoffeeMachine {
  constructor(beans: number, private sugar: AutomaticeSugarMixer) {
    super(beans)
  }
  makeCoffee(shots: number): CoffeeCup {
    const coffee = super.makeCoffee
    return this.sugar.addSugar(coffee)
  }
}
class SweetCoffeeLatteMaker extends CoffeeMachine {
  constructor(
    private beans: number,
    private milK: CheapMilkSteamer,
    private sugar: AutomaticeSugarMixer
  ) {
    super(beans)
  }
  makeCoffee(shots: number): CoffeeCup {
    const coffee = super.makeCoffee(shots)
    const sugarAdded = this.sugar.addSugar(coffee)
    return this.milk.makeMilk(sugarAdded)
  }
}
// milk, sugar  모두 기능별로 정의된 class를 통해 추출하고 결국 상속받은 CoffeeMachine의 makeCoffee 매서드를 이용해
// SweetCoffeeLatte를 생성
```
