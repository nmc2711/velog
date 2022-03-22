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

- 캐시의 유효 기간: max-age

서버의 Cache-Control 헤더의 값으로 max-age=<seconds> 값을 지정하면, 이 리소스의 캐시가 유효한 시간은 <seconds> 초가 된다.

```javascript
Note: Cache-Control max-age 값 대신 Expires 헤더로 캐시 만료 시간을 정확히 지정할 수도 있다.
```

- 캐시의 유효 기간이 지난 이후: 재검증

그렇다면 캐시의 유효 기간이 지나면 캐시가 완전히 사라지게 될까요? 그렇지는 않다. 대신 브라우저는 서버에 조건부 요청 <br/>(Conditional request)을 통해 캐시가 유효한지 재검증(Revalidation)을 수행한다.<br/>

대표적인 재검증 요청 헤더들로는 아래와 같은 헤더가 있다.<br/>

If-None-Match: 캐시된 리소스의 ETag 값과 현재 서버 리소스의 ETag 값이 같은지 확인.<br/>
If-Modified-Since: 캐시된 리소스의 Last-Modified 값 이후에 서버 리소스가 수정되었는지 확인.<br/>
위의 ETag 와 Last-Modified 값은 기존에 받았던 리소스의 응답 헤더에 있는 값을 사용한다.<br/>

재검증 결과 캐시가 유효하지 않으면, 서버는 [200 OK] 또는 적합한 상태 코드를 본문과 함께 내려준다. 추가로 HTTP 요청을 <br/>보낼 필요 없이 바로 최신 값을 내려받을 수 있기 때문에 매우 효율적이다.

- no-cache와 no-store
  Cache-Control에서 가장 헷갈리는 두 가지 값이 있다면 바로 no-cache 와 no-store 이다. .<br/>이름은 비슷하지만 두 값의 동작은 매우 다르다.

no-cache 값은 대부분의 브라우저에서 max-age=0 과 동일한 뜻을 가진다..<br/> 즉, 캐시는 저장하지만 사용하려고 할 때마다 서버에 재검증 요청을 보내야 한다..<br/>

no-store 값은 캐시를 절대로 해서는 안 되는 리소스일 때 사용한다. .<br/>캐시를 만들어서 저장조차 하지 말라는 가장 강력한 Cache-Control 값이다. .<br/>no-store를 사용하면 브라우저는 어떤 경우에도 캐시 저장소에 해당 리소스를 저장하지 않는다다. 😉
