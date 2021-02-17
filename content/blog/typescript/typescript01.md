---
title: '[ Ch # 01 ] TypeScript'
date: 2021-01-27 18:21:13
category: 'typescript'
draft: false
---

## 타입스크립트 Study #01

Typed JavaScript at Any Scale.

작업할 폴더를 하나만들어보자.
mkdir typescript

```ts
// main.ts
console.log('hello world !')
```

```js
// index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script src="./main.ts" />
    // 작성한 ts파일을 import 해본다면?
    <title>Document</title>
  </head>
  <body></body>
</html>
```

![](./images/typeerror1.png)

다음과 같은 에러가 발생한다.

즉, 브라우저는 ts파일 그 자체를 읽을순없다. 따라서 컴파일러 툴을 이용하여 ts파일을 js로 컴파일화 해줘야한다.

```
tsc main.ts
```

js 파일이 떨궈지며 읽혀진다.

이러한 컴파일 작용이 필요하다.

2장부터는 타입을 지정해가며 해보려고한다.
