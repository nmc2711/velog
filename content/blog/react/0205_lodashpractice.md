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

> map

```tsx
* map 객체로 구성된 배열에서 객체의 특정 키에대한 값을 추출할 때 사용.

animals = _.map(animals,'cage')
console.log(animals)
```

> every

```tsx
* every 통해 해당값 또는 모든값을 추적하여 일치여부를 판단하거나 존재 여부를 확인한다.
animals = _.every(animals,'age',4)
console.log(animals)

// return ... true

```

#### debounce in React

> debounce && throttle

```tsx
* debounce 연이어 호출되는 함수들 중 마지막 함수(또는 제일 처음)만 호출하도록 하는 기법이다.
* 예를 들어 검색창에 검색어를 입력할 때, 검색어 입력완료 후 특정시간 이후에만 자동검색을 수행하거나 API호출을 발생시킨다.
* debounce는 이벤트가 끝날 때까지 기다렸다가 시작된다는 점, throttle은 이벤트가 시작되면 일정 주기로 계속 실행한다는 점이 다르다.

import React, { useCallback, useState } from 'react'
import _ from 'lodash'

const App = () => {
  const [state, setState] = useState('')
  const [debouncedState, setDebouncedState] = useState('')

  const handleChange = event => {
    setState(event.target.value)
    debounce(event.target.value)
  }

  const debounce = useCallback(
    _.debounce(_searchVal => {
      setDebouncedState(_searchVal)
      // send the server request here 이부분에 API콜과 같은 비동기상황을 재현한다면
      // 1초의 시간차를 두어 발생시킬수 있다.
    }, 1000),
    []
  )

  return (
    <div className="App">
      <form>
        <label>
          Search:
          <input type="text" onChange={handleChange} />
        </label>
      </form>
      <p>Search Value: {state}</p>
      <p>Debounced Value: {debouncedState}</p>
    </div>
  )
}
export default App
```
