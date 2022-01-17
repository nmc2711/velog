---
title: 'WEB 성능 최적화 정리'
date: 2022-01-07 16:21:13
category: 'development'
draft: false
---

## 웹 성능 최적화란 무엇일까?

웹 개발을 하면서, 아.. 성능 최적화 해야하는데 이러한 고민을 해본적이 있을것이다. <br />
큰 규모의 프로젝트에서 <b>성능 최적화</b>는 굉장히 중요하다. <br />
최근 웹 개발은 ajax 통신, 화려한 UI등 많은 기능을 담으면서 점점 무거워 지고 있다. <br />
무거워진 웹은 긴 로딩 시간으로 UX에 안 좋은 영향을 끼친다. <br />
사용자의 불편함은 서비스 이탈에 있어, 치명적이므로 돌아보는 과정이 필요하다. <br />

먼저, 성능 최적화를 이해하기 위해서는 <b>브라우저의 로딩 과정</b>을 확인할 필요가있다.

### 브라우저 로딩 과정

브라우저는 웹 페이지에 필요한 리소스를 내려받고 해석한 다음 여러 계산 과정을 거쳐 콘텐츠를 화면에 보여준다. <br />
이러한 행위를 <b>브라우저의 로딩 과정</b>이라고 한다. <br />
(다운로드, 파싱, 스타일, 레이아웃, 페인트, 합성의 세부 단계) <br />

1.  <b>파싱</b> <br />
    브라우저는 웹 페이지를 로드하면 가장 먼저 html 파일을 <b>다운로드</b> 한다. <br />
    파싱은 <b>다운로드한 html을 해석해 DOM 트리를 구성</b>하는 단계이다. <br />
    파싱 중 `<script />, <link />, <img />`를 발견하면 각 리소스를 요청하고 다운로드 한다. <br />
    html 또는 리소스에 CSS가 포함된 경우에는 CSSDOM 트리 구성 작업도 함께 진행한다. <br /><br />

    - DOM 트리 구성 <br />

    ```js
     // 다운로드한 html
     <!DOCTYPE html>
     <html lang="kr">
       <head>
         <meta name="viewport" content="width=device-width,initial-scale=1">
         <link href="style.css" rel="stylesheet">
         <title>Document</title>
       </head>
       <body>
         <p>Hello <span>web performance</span> students!</p>
         <div><img src="img.jpg"></div>
       </body>
     </html>
    ```

    다운로드한 html 파일을 보면, 파싱이 일어나면 html을 해석하여 DOM을 생성한다.<br />
    각 DOM 객체를 트리 데이터 구조로 연결해 부모-자식 관계를 갖도록 만든다. <br />
    `<body>, <p>, <div>`등 각 태그가 DOM 트리의 노드로 생성되고 자식 노드를 참조한다.<br />

    ![](./images/dom.png)

    <h6>https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model?hl=ko</h6><br />

    - CSSDOM 트리 구성 <br />

    ```js
    body { font-size: 16px }
    p { font-weight: bold }
    span { color: red }
    p span { display: none }
    img { float: right }
    ```

    다음과 같이 외부 스타일시트 파일이나 내부 스타일시트가 포함되어 있을 경우, css를 해석해 cssDOM 트리를 구성한다.<br />
    `<b>, <p>, <span>`등 선택자가 노드로 생성되고 각 노드는 스타일을 참조한다.<br />

    ![](./images/cssdom.png)

    <h6>https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model?hl=ko</h6><br />

2.  <b>스타일</b> <br />
    스타일 단계에서는 파싱 단계에서 생성된 DOM, CSSDOM 트리를 통해 스타일을 매칭시켜 랜더 트리를 구성한다. <br />
    ![](./images/rendertree.png) <br />

    <h6>https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model?hl=ko</h6><br />

3.  <b>레이아웃</b> <br />
    레이아웃 단계에서는 노드의 정확한 위치와 크기를 계산한다. <br />
    노드의 정확한 크기와 위치를 파악하기 위해 루트부터 노드를 순회하면서 계산하고, 레이아웃 결과로 각 노드의 정확한 위치와 <br />
    크기를 픽셀값으로 렌더트리에 반영한다. 아래는 레이아웃 전/후 과정을 보여준다. 만약 css에서 크기 값을 %로 지정하였다면, 레이아웃 단계를 거친 후 %값은 계산되고 측정 가능한 픽셀 단위로 변환된다.<br />
    ![](./images/layout.png) <br />

4.  <b>페인트</b> <br />
    이전 레이아웃 단계에서 계산된 값을 이용해 렌더트리의 각 노드를 화면상의 실제 픽셀로 변환한다. <br />
    이때 위치와 관계없는 css속성(색상, 투명도 등)을 적용한다. 그리고 픽셀로 변환된 결과는 포토샵의 레이어처럼 생성되어 개별 레어로 관리된다. <br />
    단, 각각의 엘리먼트가 모두 레이어가 되는 것은 아니다. 이 과정을 페인트라고 한다. <br /><br />

5.  <b>합성 그리고 렌더</b> <br />
    페인트 단계에서 생성된 레이어를 합성하여 스크린을 업데이트한다. <br />
    합성과 렌더 단계가 끝나면 화면에서 웹 페이지를 볼 수 있다. <br /><br />

!! 지금까지 브라우저 로딩 과정 중 <b>렌더링</b>을 알아 보았다. (스타일 -> 레이아웃 -> 페인트 -> 합성)

### 레이아웃과 리페인트

![](./images/reflow.png) <br />

렌더링 과정은 상황에 따라 반복하여 발생할 수 있다. <br />
스타일 단계에서 구성되는 렌더 트리는 js에 의해 DOM트리, CSSDOM 트리가 변경될 때 재구성 된다. <br />
DOM이 추가/삭제되거나 요소에 <b>기하적인 영향</b>(높이, 넓이, 위치)를 주는 CSS 속성값을 변경하는 경우, 렌더 트리가 <b>재구성</b> 된다. <br />
즉, 레이아웃부터 이후 가정을 다시 수행하며 이것을 <b>레이아웃</b> 또는 <b>리플로우</b> 라고 한다.<br />

![](./images/repaint.png) <br />

레이아웃(리플로우)은 요소에 기하적인 영향을 주는 CSS 속성값을 변경할 때 발생한다고 하였는데, <br />
반대로 영향을 주지 않는 css 속성값을 변경하면 레이아웃 과정을 건너뛴다. 페인트과정 부터 수행하며 이를 <b>리페인트</b>라고 한다.<br />
레이아웃이 일어나면 전체 픽셀을 다시 계산해야 하므로 부하가 크다. <br />
반면 <b>리페인트</b>는 이미 계산된 픽셀값을 이용해 화면을 그리기 때문에 레이아웃에 비해 부하가 적다. <br />

#### 요소에 기하적인 영향을 주는 CSS 속성값

```js
myBox.style.width = '500px'
```

![](./images/cssup.png) <br />
!! 리페인트 발생 <br />
CSS 속성값 : height, width, left, top, font-size, line-height 등 <br />

#### 요소에 기하적인 영향을 주지 않는 CSS 속성값

```js
myBox.style.backgroundColor = 'orange'
```

![](./images/cssdown.png) <br />
!! 레이아웃(리플로우) 발생 <br />
CSS 속성값 : background-color, color, visibility, text-decoration 등 <br />

### \*\* 해결책 1) 위와같이 레이아웃이 발생하면 실행 시간만큼 렌더링 시간도 늘어나게 된다.

### 따라서 불필요한 레이아웃(리플로우)이 발생하지 않도록 신경 써야 한다.
