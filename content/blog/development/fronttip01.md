---
title: '프론트엔드 개발을 하며 유용한 팁 - 01'
date: 2021-10-31 16:21:13
category: 'development'
draft: false
---

\* 프론트엔드 개발을 하며 유용한 팁 - 01

### @Font. TTF vs WOFF 차이

---

프로젝트의 초기 혹은 디자인 시스템의 변경으로 전역 폰트에 수정이 필요할경우가 있다. <br />

그러한 과정중 TTF, WOFF 등의 폰트에대한 정의를 볼 수 있는데 그것에 대해 알아보자. <br />

1. TTF ? PC에 설치하여 사용하는 폰트 <br />

2. WOFF ? Web상에서 사용하는 글꼴
   (OTF와 TTF를 압축해 웹에서 사용할 수 있게 만든 포맷) <br />

## 나올수있는 의문?

<b>클라이언트 스타일 설정할시 TTF, WOFF 비슷한
역활인것으로 예상되는데 왜 대부분 두가지 포맷을
font-face에 설정하는것인가?</b> <br /><br />

<b>폰트 꽤 큰 용량의 코드이다. 따라서 폰트를 불러옴에 있어서 환경에 따른 시간소요가 생길수있다.
따라서 두가지를 비교하여 로컬과 웹에서 폰트를 불러드리는 방법을 옵셔널하게 지정한다.
(코드 순서를 바꿔주어 우선순위 적용 등)
다른이점으로는 "TFF"를 선언해줄시 구형브라우저를 지원한다는 장점이 있다. </b>

```예시코드
  url('${font.src}-${weightStr}.woff2') format('woff2'),
  url('${font.src}-${weightStr}.woff') format('woff'),
  url('${font.src}-${weightStr}.ttf') format('truetype')
```
