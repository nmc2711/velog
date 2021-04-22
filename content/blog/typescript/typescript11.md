---
title: 'TypeScript Object Oriented Programming Inheritance'
date: 2021-03-01 18:21:13
category: 'typescript'
draft: false
---

## Inheritance 을 통한 다양한 기능구현

```tsx
type CoffeeCup = {
  shots: number
  hasMilk: boolean
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
  // 다른 클래스를 extends를 통한 상속을 받을때는 상속받는 constrouctor가 private이면 안된다.(public이나 protected를 사용)

  private steamMilk(): void {
    console.log(`steaming some milk...`)
  }

  // 만약 상속받은 클래스에도 생성자를 써야한다면
  constructor(beans:number,public readonly serialNumber:string) {
      super(beans)
      // 부모에서 넘겨받아야할 beans를 넣어주고 super을 통한 생성이 필요하다
  }

  makeCoffee(shots: number): CoffeeCup {
    const coffee = super.makeCoffee
    // super을 통해 부모클래스의 함수사용
    this.steamMilk()
    return {
      ...coffee,
      hasMilk: true,
    }
  }
}
const latteMachine = new CaffeLatteMachine(23,'시리얼넘버123')
const coffee = latteMachine.makeCoffee(1)
console.log(coffee)
// grinding beans for 1
// '커피머신 데우는중 ..... fire...!'
// `커피 내리는중 ....1 shots...`
---부모에서 사용하는 함수를 사용하고
// streaming some milk ...
//{shots:1,hasMilk:true}
---상속받은 클래스내에서 overwirte

console.log(latteMachine.serialNumber)
// 라떼머신속 컨스트럭쳐에서 생성한 시리얼넘버에만 접근가능
```
