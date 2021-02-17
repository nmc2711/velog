---
title: '[ Ch # 04 ] TypeScript Basic Type'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

```tsx
//Array
let fruits: string[] = ['tomato', 'banna']
let scores: Array<number> = [1, 2, 3]
function printArray(fruits: readonly string[]) {
  // readonly 를 쓰게 되면
  // (x) fruits.push('orange') 배열을 변경할수없다.
  // 두 타입선언의 차이점이라고 하면 Array<number>는 readonly를 사용할수없다.
}

//Tuple ->interface,typealias,class 대체 해서 보통사용
let student: [string, number]
student = ['name', 1231]
```
