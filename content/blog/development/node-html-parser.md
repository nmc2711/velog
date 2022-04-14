---
title: 'html-react-parser'
date: 2022-01-15 16:21:13
category: 'development'
draft: false
---

api 호출로 data 출력할때 <br />
html를 넣어 바로 렌더링 시킬때<br />
사용한다.<br />

html-react-parser 라이브러리를 사용할 수 있다.<br />
라이브러리를 이용해 html를 react의 문법인 jsx 로 변환한다.<br />

```javascript
import Parser from 'html-react-parser'; //1. import 삽입
...
return (<div>{Parser(title)}</div>) //2. html 코드
```
