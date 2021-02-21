---
title: '[ Ch # 06 ] TypeScript Basic Type'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

- Type Inference ? 타입추론

```tsx
let text = 'hello'
text = 'ok'
(x) text = 1
//함수의 파라미터쪽을 자세히보면 ...이 표시되어있는 인자쪽에 any의 타입값이 기본세팅되어있다.
function print(message:string){
    console.log(message)
}
print('hello');
(x) print(1);

function add(x:number,y:number) {
    return x + y; // 자동으로 숫자타입 추론
}
const result = add(1,2) // type? number

// but ... 정확한 타입을 명시하자는 취지와 약간 상이됨 따라서 간단한 함수라도 return 값 타입지정 습관기르기
function add(x:number,y:number):number {
    return x + y; // 자동으로 숫자타입 추론
}
```

- Type Asserssion ? 타입추론강요 (비추.. 피하는 스킬 습득 및 인지)

```tsx
function jsStrFn(): any {
  return 'string'
}
const result = jsStrFn()
// type을 확신하여 알고 있을때 사용
console.log((result as string).length) //5
console.log((<string>result).length) //5

function jsStrFnV2(): any {
  return 3
}
const resultV2 = jsStrFnV2()
console.log((result as string).length) // error가 발생하지 않지만 undefined를 할당함

const wrong: any = 5
console.log((wrong as Array<number>).push(1)) // error가 발생하고 죽음 위험

function findNum(): number[] | undefined {
  return undefined
}

const numbers = findNum()
numbers.push(2) // warnning
numbers!.push(2) // !를 통해 배열이라는 절대확신

const button = document.querySelector('class')! // !를 통한 절대확신
```
