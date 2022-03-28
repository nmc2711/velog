---
title: 'Emotion'
date: 2022-03-15 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">emotion이란?</h3>

emotion은 자바스크립트로 css를 다룰 수 있게 해주는 css-in-js의 아이디어를 기반으로 한 도구이다.<br /> 예전 레거시 웹프로그래밍을 경험해본 사람이라면 아마 css를 사용하다가 많은 문제를 겪어본 경험이 있을 것이다.

이를테면 전체 페이지의 디자인을 파란색에서 노란색으로 변경한다고 했을 때, css 내부에 존재하는 많은<br /> blue 색상을 찾아다니며 체크하고 yellow로 바꾸어보고 하는 과정을 거쳐야 했다.

만일, css에서 변수를 사용할 수 있다면, themeColor라는 변수를 만들고 themeColor만 blue에서 <br />yellow로 바꾸면 될 일인데 말이다.

사실 이렇게 변수를 사용하는 것은 sass라는 컴파일 방식의 css를 사용하면 되는 일이긴 하다. 하지만 더욱 난감하고 귀찮은 케이스는 매 페이지마다 일부분만 독립적인 스타일을 사용하고, 나머지는 공통된 스타일을 그대로 적용하고 싶다면? (Scoping 문제)

이를테면 같은 사이트 내부에서 페이지 A에서는 버튼이 노란색이고 싶고, B에서는 파란색이고 싶다면? 대신 나머지 속성은 모두 동일하게 가져가고 싶다면? 무식하게 inline-css에 각 페이지마다 일일이 다르게 적용되는 속성만 정의해줄 수도 있을테고, 또 하나의 클래스를 만들어서 적용할 수도 있을 것이다.

이렇게 하다가 많은 클래스가 생기면 나중에는 class="a-class b-class c-class"와 같이 클래스의 여러 중첩이 일어나서 복잡성(complexibility)이 매우 높아지게 된다. 그렇게 되면 코딩한 본인도 매우 헷갈리는 경우가 생기고, html파일 자체의 내용도 매우 길어져서 헷갈리게 된다.

이렇게 비효율적인 작업을 많이 경험한 개발자들은 css에서의 여러가지 불편함을 해소하기 위해 css-in-js라는 아이디어를 내놓았다.

css에 js를 결합한다는 아이디어 하나만으로 css를 아주 편하게 관리할 수 있게 됐다.

사람들이 요즘 가장 많이 쓰는 방법은 styled-components를 이용한 방법으로, css를 일일이 주기 보다는 어떠한 커스텀태그를 만들어서 그 안에 내부적으로 css를 적용시켜놓는 것이다.

이를테면 <redbutton>Red Button</redbutton>이라는 태그를 입력하면 css를 따로 주지 않아도 빨간 버튼이 나오는 방식이다.

<br />

// sample

```javascript
import { css, jsx } from '@emotion/react'

const divStyle = css`
  background-color: hotpink;
  font-size: 24px;
  border-radius: 4px;
  padding: 32px;
  text-align: center;
  &:hover {
    color: white;
  }
`

export default function App() {
  return <div css={divStyle}>Hover to change color.</div>
}
```