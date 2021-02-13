---
title: 'Algorithm Challenge 01'
date: 2020-07-02 16:21:13
category: 'javascript'
draft: false
---

## 문제 01 암호해독!

모든 알고리즘을 해독할 수 있는 알고리즘 7 원석를 보유한 알고리즘 제왕 **파이**와 **썬**은 죽기 전, 이 보물에 '암호'를 걸어 세계 어딘가에 묻어놨다고 공표하였다. 그가 남긴 문자는 아래와 같다.

OO섬으로 향하라! (Quiz:OO섬의 이름을 구하시오.)

' + -- + - + - '
' + --- + - + '
' + -- + - + - '
' + - + - + - + '

해(1)와 달(0),
Code의 세상 안으로!(En-Coding)

=> 출력조건: 문자열

---

```jsx
1. 문제파악 : 출력값 oo섬의 이름 이며 문자열이다.
2. 제공된 자료 분석 ''을통한 구분값이 배열가 유사함,1과 0을 제시 2진법의 힌트,EnCoding 여기서 약간 아리송? 일단 참고

let answer = ""
let data = [
  ' + -- + - + - ',
  ' + --- + - + ',
  ' + -- + - + - ',
  ' + - + - + - + ',
]
 `for of` 를 이용하여 전체를 돌아 각각의 key값을 분석하여 그대로 반환할것이다.
for (var item of data) {
  console.log("originItem", item);
// return ....
// originItem  + -- + - + -
// originItem  + --- + - +
// originItem  + -- + - + -
// originItem  + - + - + - +
배열을 자세히 보면 앞뒤로 공백이 존재한다. 제거해주자 !
console.log("noSpace", item.replace(/ /g, ""));
// return ....
// noSpace +--+-+-
// noSpace +---+-+
// noSpace +--+-+-
// noSpace +-+-+-+
해(1),달(0) 이라는 힌트를 주었는데 배열에서 1,0을 나타낼수있는 문자는 +,-일것이다. 1,0을 변환시켜보자.
console.log("transformItem", item.replace(/ /g, "").replace(/\+/g,'1').replace(/-/g,'0'));
// return ...
// transformItem 1001010
// transformItem 1000101
// transformItem 1001010
// transformItem 1010101
이제 여기서 살짝어려웠는데 `2진수의 숫자타입을 문자로 변환`하는게 있을까라고 생각해봐야할것이다.
MDN 을 무한 검색중 `String.fromCharCode()` 메서드는 UTF-16 코드 유닛의 시퀀스로부터 문자열을 생성해 반환 서칭
UTF-16 코드 유닛 ? type은 number일것이다.
console.log("codeItem", String.fromCharCode((parseInt(item.replace(/ /g, "").replace(/\+/g,'1').replace(/-/g,'0'))));
// return ...
//codeTransferItem J
//codeTransferItem E
//codeTransferItem J
//codeTransferItem U

여기서 정답은 문자열 반환이었으므로 위에 answer 이라는 빈 변수를 주어 += 복합연산자로 for of문 내에서 붙여서 출력하자
answer += String.fromCharCode((parseInt(item.replace(/ /g, "").replace(/\+/g,'1').replace(/-/g,'0')));
}

console.log(answer)
// return ... 'JEJU'
섬의 이름은 제주(JEJU)였던것 ?!
```
