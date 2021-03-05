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
maker.makeMachine(50)
maker.fillCoffeBeans(32)
maker.makeCoffee(6)
```

## More Detail Getter & Setter

```tsx
class User {
  firstName: string
  lastName: string
  fullName: string
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
    this.fullName = `${firstName} ${lastName}`
  }
}

const user = new User('Hanwang', 'han')
console.log(user.fullName) // Hanwang han

 만약, 여기서 생성된 객체의 constructor에 접근하여 firstName 을 변경한다면 ? 이미할당한 객체의 요소는 바뀌지않음
 user.firstName = 'david'
 console.log(user.fullName) // Hanwang han
```

이에 대한 개선 방향으론..

```tsx
class User {
  firstName: string
  lastName: string
  get fullName(): string {
    return `${firstName} ${lastName}`
  }
  // constructor 에서 생성하는 것이 아닌 get을 통한 반환으로 fullName을 생성한다.
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
  }
}

const user = new User('Hanwang', 'han')
console.log(user.fullName) // Hanwang han
user.firstName = 'david'
console.log(user.fullName) // david han
```

getter와 setter 함수로 코드를 추가해보고 constructor에 속성부여로 코드를 간결하게 해보자.

```tsx
class User {
  get fullName(): string {
    return `${firstName} ${lastName}`
  }

  private internalAge = 4

  get age(): number {
    return this.internalAge
  }

  set age(num: number) {
    if (num < 0) {
      alert('1살이상이어야해...')
    } else {
      this.internalAge = num
    }
  }

  constructor(private firstName: string, public lastName: string) {
    // 생성자에서 private와 같은 속성을 부여함으로 자동으로 멤버변수로 설정 가능하다.
  }
}

const user = new User('Hanwang', 'han')
user.age = 6
//setter을 통한 설정
console.log(user.age) // 6
//getter을 통한 접근
```
