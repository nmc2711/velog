---
title: '[ Ch # 05 ] lodash in React '
date: 2021-02-05 18:21:13
category: 'react'
draft: false
---

## lodash의 사용법과 React 안에서 적용

lodash는 데이터 구조를 조작하는 데 사용되는 JavaScript library 이다. <br />
여러가지 유용한 유틸리티 기능을 제공하며 복잡한 자료구조 를 컨트롤하는데 매우 쉽고 편리하게 사용할 수 있도록 도와준다.

```sh

npm i lodash

import _ from lodash;

```

기본 사용법 숙지해보기 !

```tsx
let animals = [
  { name: 'tiger', age: 4, gender: 'male', cage: '3sector' },
  { name: 'zebra', age: 1, gender: 'female', cage: '1sector' },
  { name: 'lion', age: 8, gender: 'female', cage: '2sector' },
  { name: 'hipo', age: 3, gender: 'male', cage: '4sector' },
]
```

### !Methods

> find

```tsx
animals = _.find(animals, item => item.name === 'lion')
console.log(animals)
// return ... {name:'lion',age:8,gender:'female',cage:'2sector'}

* anOther example

animals = _.find(animals,{name:'lion'})
console.log(animals)
// return ... {name:'lion',age:8,gender:'female',cage:'2sector'}

```

> filter

```tsx
animals = _.filter(animals, item => item.age < 4)
console.log(animals)
// return ... [{name: "zebra" ,age: 1,gender: "female",cage: "1sector"},{name: "hipo",age: 3,gender: "male",cage: "4sector"}]
```

> sum & sumBy

```tsx
* sumBy 함수를 이용하여 배열의 각 요소를 통해 반복해서 원래의 배열의 합을 계산하는 데 사용된다.(루프를 돔)

animals = _.sumBy(animals, item => item.age)
animals = _.sumBy(animals, _.property('age'))
animals = _.sumBy(animals, 'age')
// return ... 16
```

> maxBy

```tsx
* maxBy 배열의 각 요소를 통해 반복해서 원래의 배열에서 최대 값인 요소를 반환한다.

animals = _.maxBy(animals, 'age')
// return ... {name:'lion',age:8,gender:'female',cage:'2sector'}
```

> chain , sortBy , map

```tsx
* chain은  lodash wrapper 인스턴스를 만들어 주므로 연속해서 lodash 함수를 사용할때 유용하게 사용할 수 있다. 메서드 체인을 사용후에 최종적으로 값을 얻기 위해서는 value()를 호출해서 값을 얻어 낼 수 있다.

animals = _.chain(animals)
  .sortBy('name')
  // 알파벳 순으로 정렬
  .map((item, idx) => `${idx + 1}. ${item.name}`)
  // 맵을 통한 name 요소의 반환
  .value()

  console.log(animals)
  // return ... ["1. hipo", "2. lion", "3. tiger", "4. zebra"]
```

> uniqBy

```tsx
* uniqBy 중복된 배열값을 제외하고 첫번째 고유값을 반환하는 매서드
animals = _.uniqBy(animals, 'gender')

console.log(animals)
// return ... [{name: "tiger",age: 4,gender: "male",cage: "3sector"},{name: "zebra",age: 1,gender: "female",cage: "1sector"}]
```

> groupBy

```tsx
* groupBy 해당 키값과 일치하는 항복을 묶어서 재반환한다. 그룹화 된 값의 순서는 컬렉션에서 발생하는 순서에 따라 결정된다.
animals = _.groupBy(animals, 'gender')
return ...
  [
    [
      {name: "tiger",age: 4,gender: "male",cage: "3sector"},
      {name: "hipo",age: 3,gender: "male",cage: "4sector"}
      ],
    [
      {name:"zebra",age: 1,gender: "female",cage: "1sector"},
      {name: "lion",age: 8,gender: "female",cage: "2sector"}
    ]
  ]
```

> flatten

```tsx
* flatten 배열을 1 depth 로 평면화하는 데 사용됩니다.
let newAnimals =  [
    [
      {name: "tiger",age: 4,gender: "male",cage: "3sector"},
      {name: "hipo",age: 3,gender: "male",cage: "4sector"}
      ],
    [
      {name:"zebra",age: 1,gender: "female",cage: "1sector"},
      {name: "lion",age: 8,gender: "female",cage: "2sector"}
    ]
]
newAnimals = _.flatten(newAnimals)

console.log(newAnimals)

// return ...
[
  { name: 'tiger', age: 4, gender: 'male', cage: '3sector' },
  { name: 'zebra', age: 1, gender: 'female', cage: '1sector' },
  { name: 'lion', age: 8, gender: 'female', cage: '2sector' },
  { name: 'hipo', age: 3, gender: 'male', cage: '4sector' },
]
```
