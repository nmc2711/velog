---
title: 'JWT (JSON WEB TOKEN)'
date: 2021-04-01 16:21:13
category: 'development'
draft: false
---

## JWT

JWT 는 JSON Web Token의 약자로 전자 서명 된 URL-safe (URL로 이용할 수있는 문자 만 구성된)의 JSON입니다.<br />

JWT는 서버와 클라이언트 간 정보를 주고 받을 때 Http 리퀘스트 헤더에 JSON 토큰을 넣은 후 서버는 별도의 인증 과정없이 헤더에 포함되어 있는 JWT 정보를 통해 인증합니다. <br />
이때 사용되는 JSON 데이터는 URL-Safe 하도록 URL에 포함할 수 있는 문자만으로 만듭니다.<br />

### JWT PROCESS

1. 사용자가 ID 와 PASSWORD를 입력하여 로그인을 시도한다.
2. 서버는 요청을 확인하고 SECRET KEY를 통해 ACCESS TOKEN 발급
3. JWT 토큰을 클라이언트에 전달한다.
4. 클라이언트에서 API를 요청할때 클라이언트가 Authorization header에 Access token을 담아서 보냅니다.
5. 서버는 JWT Signature를 체크하고 Payload로부터 사용자 정보를 확인해 데이터를 반환합니다
6. 클라이언트의 로그인 정보를 서버 메모리에 저장하지 않기 때문에 토큰기반 인증 메커니즘을 제공합니다.
   JWT에는 필요한 모든 정보를 토큰에 포함하기 때문에 데이터베이스과 같은 서버와의 커뮤니케이션 오버 헤드를 최소화 할 수 있습니다.<br />
   Cross-Origin Resource Sharing (CORS)는 쿠키를 사용하지 않기 때문에 JWT를 채용 한 인증 메커니즘은 두 도메인에서 API를 제공하더라도 문제가 발생하지 않습니다.<br />
   일반적으로 JWT 토큰 기반의 인증 시스템은 위와 같은 프로세스로 이루어집니다.<br />
   처음 사용자를 등록할 때 Access token과 Refresh token이 모두 발급되어야 합니다

### 장점

JWT 의 주요한 이점은 사용자 인증에 필요한 모든 정보는 토큰 자체에 포함하기 때문에 별도의 인증 저장소가 필요없다는 것입니다.

- URL 파라미터와 헤더로 사용
- 트래픽 대한 부담이 낮음
- REST 서비스로 제공 가능

### 단점

토큰은 클라이언트에 저장되어 데이터베이스에서 사용자 정보를 조작하더라도 토큰에 직접 적용할 수 없습니다
