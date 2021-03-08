---
title: 'TypeScript Object Oriented Programming Interface & Composition Deep'
date: 2021-03-06 18:21:13
category: 'typescript'
draft: false
---

## Interface & Composition 심화!

/\* 기능의 기준점이 되는 interface MilkFrother,SugarProvider를 구성하여
class를 다양화 하고 결국 CoffeeMachine 이라는 객체 속에서 다양화된 기능을 처리

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

  constructor(
    coffeeBeans: number,
    private milK: MilkFrother,
    private sugar: SugarProvider
  ) {
    this.coffeeBeans = coffeeBeans
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
    const coffee = this.extract(shots)
    const sugarAdded = this.sugar.addSugar(coffee)

    return this.milk.makeMilk(sugarAdded)
  }
}
// interface를 통한 매서드 분리로 클래스 종류 다양화
interface MilkFrother {
  makeMilk(cup: CoffeeCup): CoffeeCup
}
interface SugarProvider {
  addSugar(cup: CoffeeCup): CoffeeCup
}

// 커피 거품기
class CheapMilkSteamer implements MilkFrother {
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

class FancyMilkSteamer implements MilkFrother {
  private steamMilk(): void {
    console.log(`Fancy steaming some milk...`)
  }
  makeMilk(cup: CoffeeCup): CoffeeCup {
    this.steamMilk()
    return {
      ...cup,
      hasMilk: true,
    }
  }
}

class ColdMilkSteamer implements MilkFrother {
  private steamMilk(): void {
    console.log(`cold steaming some milk...`)
  }
  makeMilk(cup: CoffeeCup): CoffeeCup {
    this.steamMilk()
    return {
      ...cup,
      hasMilk: true,
    }
  }
}

class NoMilk implements MilkFrother {
  makeMilk(cup: CoffeeCup): CoffeeCup {
    return cup
  }
}

// 설탕 제조기
class CandySugarMixer implements SugarProvider {
  private getSugar(): void {
    console.log(`getting some sugar form candy...`)
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
class SugarMixer implements SugarProvider {
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

class NoSugar implements SugarProvider {
  addSugar(cup: CoffeeCup): CoffeeCup {
    return cup
  }
}

//milk
const cheapMilkMaker = new CheapMilkSteamer()
const fancyMilkMaker = new FancyMilkSteamer()
const coldMilkMaker = new ColdMilkSteamer()
const noMilk = new NoMilk()
//sugar
const candySugar = new CandySugarMixer()
const sugar = new SugarMixer()
const noSugar = new Nosugar()
//
const sweetCandyMachine = new CoffeeMachine(12, noMilk, candySugar)
const sweetMachine = new CoffeeMachine(12, noMilk, sugar)

const latteeMachine = new CoffeeMachine(12, cheapMilkMaker, noSugar)
const coldLatteMachine = new CoffeeMachine(12, coldMilkMaker, noSugar)

const sweetLatteMachine = new CoffeeMachine(12, cheapMilkMaker, candySugar)
```
