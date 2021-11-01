---
title: '프론트엔드 개발을 하며 유용한 팁 - 02'
date: 2021-11-01 16:21:13
category: 'development'
draft: false
---

\* 프론트엔드 개발을 하며 유용한 팁 - 02

### @user. 클라이언트의 경로 파악하기

---

프로젝트를 진행하게되면 클라이언트의 접속 경로를 파악해야하는 경우가 무수히 많다. <br />

여러가지 접근방법이 있겠지만, 대부분의 웹개발자들은 "navigator.userAgent"를 이용할것이다.<br />

<b>BUT? 브라우저 엔진(크롬, firefox 등)에서 제공하던 navigator.userAgent 메서드가 2020 하반기 이후로
더이상 업데이트를 진행하지 않는다고 한다.(프리징)</b> <br />

<b>WHY? 이유는 많은 정보량이 담겨 있어 “개인정보 침해“ 우려이다. 필터없이 스트링으로 모든 유저정보를 내어주는 문제.</b>

```
  ** 기존의 유저의 경로 접근의 일반적인방법

  const root = navigator.userAgent;

  // 반환정보

  'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36'


  const mobile = root.match(/.*(iPhone|iPod|Android|Windows CE|BlackBerry|Symbian|Windows Phone|webOS|Opera Mini|Opera Mobi|POLARIS|IEMobile|lgtelecom|nokia|SonyEricsson).*/g);

  해당 문자열이 위의 필터와 매치되는지로 경로를 파악
```

<b>그렇다면 대용방법이 뭔데?</b>

그 대용으로 "User-Agent Client Hints"를 사용할수 있다.
User-Agent string을 세분화해 “Object 형식“으로 나타낸 User-Agent Data를 사용할 수 있다.

```
  navigator.userAgentData

  NavigatorUAData 
  {brands: Array(3), mobile: false, platform: 'macOS'},
  brands: (3) [{…}, {…}, {…}],
  mobile: false,
  platform: "macOS",
  [[Prototype]]: NavigatorUAData,

```

## 나올수있는 의문?

<b>필요정보만 가져와서 사용하게 되므로 개인정보보호에 적합하고 객체 형식이라 개발에 유용한 장점이있다. <br />
ps) 보통 클라이언트에서 해당 메서드를 사용하는이유는 현재 클라이언트가 접속하고있는 플래폼이
모바일 인지 웹인지의 따른 반응형이나 스타일 혹은 플래폼에 따른 코드 분기 등에 있다. </b>
