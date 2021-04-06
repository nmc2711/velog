---
title: 'http vs https'
date: 2021-04-01 16:21:13
category: 'development'
draft: false
---

# http vs https ?

HTTP는 HyperText Transfer Protocol의 약자로, <br />
web browser와 web server가 통신할 때 사용하는 통신규칙입니다.

즉, web browser와 web server 사이에서 통신할 때 서로가 알아 들을 수 있는 일종의 약속인 message 필요한데, <br ?>
이 역할을 HTTP가 하는 것이죠. 그리고 이 message는 web browser가 요청할 때 보내는 request message, web server가 응답할 <br />때 보내는 response message 로 구분될 수 있습니다. <br />

웹이 점점 발전하면서 성능, 보안 등의 문제가 대두되었고 현재 `웹 통신규약의 표준은 HTTPS`가 되었습니다.

그 이유? <br />

1. 보안
   HTTPS에서 s는 Over Secure Socket Layer의 약자입니다
   HTTPS의 가장 큰 장점은 `보안`입니다 <br />
   HTTP는 web browser와 web server 사이에서 전송되는 데이터의 정보들이 암호화 되지 않습니다. 따라서 전송하는 과정에서 해커로부터의 데이터 도난에 취약하다고 할 수 있습니다. 그러나`HTTPS는 데이터 정보들을 암호화`함으로써, 해커가 데이터를 <br />도난 한다고 해도 정보를 해독 할 수 없습니다.

   암호화는 SSL 인증을 통해 이루어집니다. SSL 인증서는 클라이언트와 서버간의 통신을 제3자가 보증해주는 전자화된 문서입니다. `발급받은 SSL 인증서를 서버에 설치하는 방식으로 SSL 인증서를 적용`할 수 있습니다.

2. SEO
   구글은 몇년 전부터 HTTPS를 사용하는 웹사이트에 대해서 검색 순위 결과에 약간의 가산점을 주는 정책을 취해오고 있습니다. <br /> 이는 HTTPS를 사용하지 않을 경우 검색결과에서 하단으로 밀려나는 것과 마찬가지이죠. 또한 구글은 HTTP를 사용하는 사이트에 크롬으로 접속하면 URL 창에 “안전하지 않음”이라는 경고를 띄우는 패널티를 적용하고 있습니다.<br />
   HTTPS 보급에 앞장서는 구글의 이러한 행보도 결국 안전한 인터넷 환경(보안환경)을 위한 것이라고 할 수 있습니다.
