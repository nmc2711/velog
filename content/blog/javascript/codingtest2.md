---
title: '1일 1문제 코딩테스트'
date: 2020-03-13 16:21:13
category: 'javascript'
draft: false
---

1. 삼각형 판별하기

길이가 서로 다른 A,B,C 세 개의 막대 길이가 주어지면 이 세 막대로 삼각형을 만들 수 있으면 'YES'를 출력하고,
만들 수 없다면 "NO"를 출력한다.

```js
function solution(A, B, C) {
  // 길이에 대한 삼각형 조건 => 한변의 길이가 두변의 합보다 크거나 같으면 안된다.
  let answer = 'YES'
  let max
  let sum = A + B + C

  if (A > B) {
    max = A
  } else {
    max = B
  }
  if (C > max) max = C
  if (sum - max <= max) {
    answer = 'NO'
  }

  return answer
}
console.log(solution(6, 7, 11))
// "YES"
console.log(solution(13, 37, 17))
// "NO"
```
