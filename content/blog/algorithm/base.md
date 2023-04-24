---
title: base
date: 2023-04-25 17:04:72
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

<h3>알고리즘을 위한 Javascript</h3>

<b>입/출력 Data확인</b>

1.입력 데이터가 텍스트 파일 형태로 주어지는 경우, 파일 시스템 모듈을 사용한다. <br />
• 예를 들어 /dev/stdin 파일에 적힌 텍스트를 읽어오는 경우, 다음과 같이 코드를 작성한다. <br />
• 기능: 전체 텍스트를 읽어 온 뒤에, 줄바꿈 기호를 기준으로 구분하여 리스트로 변환하기 <br />

```javascript
let fs = require('fs')
let input = fs
  .readFileSync('input.txt')
  .toString()
  .split('\n')
console.log(input)
```

```
input.txt [입력 예시]
123
456
789 1000

input.txt [출력 예시]
['123', '456', '789 1000']
```

2.한 줄씩 입력을 받아서, 처리하여 정답을 출력할 때는 readline 모듈을 사용할 수 있다.<br />

```javascript
const rl = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout,
})
let input = []
rl.on('line', function(line) {
  // 콘솔 입력 창에서 줄바꿈(Enter)를 입력할 때마다 호출
  input.push(line)
}).on('close', function() {
  // 콘솔 입력 창에서 Ctrl + C 혹은 Ctrl + D를 입력하면 호출(입력의 종료)
  console.log(input)
  process.exit()
})
```

```javascript
//for 조건에 따라서 특정한 코드를 반복하고자 할 때 사용할수있는 코드다.
/*
- 초기문이 존재한다면 초기문이 먼저 실행됩니다.
- 조건문이 참이라면 블록 내부 코드가 실행되고, 거짓이면 반복문이 종료됩니다. - 블록 내부 코드가 실행된 뒤에 증감문이 실행됩니다.
*/
for (초기문; 조건문; 증감문) {
  // statements
}
// 1부터 100까지의 합 계산
let summary = 0
for (let i = 1; i <= 100; i++) {
  summary += i
}
console.log(summary)

//while 조건에 따라서 특정한코드를 반복하고자 할 때 사용할수있는 코드다.
/*
- while문은 조건문이 참일 때 실행되는 반복문입니다.
- 블록 내부의 코드가 실행되기 전에 참 혹은 거짓을 판단합니다. */
while (조건문) {
  // 실행되는 코드 부분
}

//Array.prototype.reduce()
//배열의 모든 원소에 대해 특정한 연산을 순차적으로 적용할 때 reduce()를 사용한다.
/*
reduce() 메서드는 배열의 각 요소에 대해 reducer 함수를 실행한 뒤에 하나의 결과를 반환합니다. reducer의 형태: (accumulator, currentValue) => (반환값)
- 배열의 각 원소를 하나씩 확인하며, 각 원소는 currentValue에 해당합니다.
- 반환값은 그 이후의 원소에 대하여 accumulator에 저장됩니다.
*/
let data = [5, 2, 9, 8, 4]
// minValue 구하기 예제
let minValue = data.reduce((a, b) => Math.min(a, b))
console.log(minValue) // 2
// 원소의 합계 구하기 예제
let summary = data.reduce((a, b) => a + b)
console.log(summary) // 28

//배열 초기화
// 직접 값을 설정하여 초기화
let arr = [8, 1, 4, 5, 7]
// 길이가 5이고 모든 원소의 값이 0인 배열 초기화
let arr = new Array(5).fill(0)

//집합 자료형
//특정한 원소의 등장여부를 파악할때 집합자료형을 효과적으로 사용할수있다.
let mySet = new Set() // 집합 객체 생성
// 여러 개의 원소 삽입
mySet.add(3)
mySet.add(5)
mySet.add(7)
mySet.add(3)
console.log(`원소의 개수: ${mySet.size}`)
console.log(`원소 7을 포함하고 있는가? ${mySet.has(7)}`)
// 원소 5 제거
mySet.delete(5)
// 원소를 하나씩 출력
for (let item of mySet) console.log(item)

//소수점 아래 특정 자리에서 반올림
//실수를 출력할때 소수점 아래 특정자리에서 반올림할수있다.
// 특정 실수에 대하여 toFixed()를 이용해 소수점 아래 둘째 자리까지 출력
let x = 123.456
console.log(x.toFixed(2))
```
