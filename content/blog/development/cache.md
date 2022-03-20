---
title: '웹 서비스 캐시'
date: 2022-03-15 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">웹 서비스 캐시 똑똑하게 다루기</h3>

이글은 토스테크의 웹 서비스 캐시 똑똑하게 다루기 아티클이 너무 맘에 들어 요약해서 정리해보았습니다. <br />
https://toss.tech/article/smart-web-service-cache <br />

- 캐시의 생명 주기

HTTP에서 리소스(Resource)란 웹 브라우저가 HTTP 요청으로 가져올 수 있는 모든 종류의 파일을 말한다. <br />
대표적으로 HTML, CSS, JS, 이미지, 비디오 파일 등이 리소스에 해당한다.

웹 브라우저가 서버에서 지금까지 요청한 적이 없는 리소스를 가져오려고 할 때, 서버와 브라우저는 완전한 HTTP 요청/응답을 <br />주고받는다. HTTP 요청도 완전하고, 응답도 완전하다. 이후 HTTP 응답에 포함된 Cache-Control 헤더에 따라 <br />받은 리소스의 생명 주기가 결정된다.
