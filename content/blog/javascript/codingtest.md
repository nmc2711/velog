---
title: '1일 1문제 코딩테스트'
date: 2020-03-12 16:21:13
category: 'javascript'
draft: false
---

1. 세 수 중 최솟값

100이하의 자연수 A,B,C를 입력받아 세 수 중 가장 작은 값을 출력하는 프로그램을 작성하세요(정렬을 사용하면 안된다.)

입력설명 : 첫 번째 줄에 100이하의 세 자연수가 입력된다.

출력설명 : 첫 번쟤 줄에 가장 작은 수를 출력한다.

```js
function solution(a, b, c) {
  let answer = ''

  if (a < b) {
    answer = a
  } else {
    answer = b
  }
  if (c < answer) {
    answer = c
  }

  return answer
}
console.log(solution(6, 5, 11))
// 1
```
