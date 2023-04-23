---
title: useUserAgent
date: 2023-04-23 17:04:72
category: hooks
thumbnail: { thumbnailSrc }
draft: false
---

`useUserAgent` 커스텀 훅은 현재 브라우저의 사용자 에이전트(User Agent) 정보를 쉽게 가져올 수 있도록 도와줍니다. <br />
user agent 정보를 사용하면 클라이언트의 브라우저, 운영 체제, 장치 등과 관련된 정보를 알 수 있습니다. <br />

```
1. getUserAgentInfo는 user agent 정보를 파싱하는 데 필요한 로직을 작성합니다.
UAParser 라이브러리를 사용하여 user agent 정보의 문자열에서 운영 체제, 브라우저, 장치 정보를 컨버팅 합니다.

2. 마운트될 때 getUserAgentInfo 함수를 호출하고 반환된 사용자 에이전트 정보를 상태에 저장합니다.

3. userAgentInfo는 아래의 속성을 반환합니다.

os: 운영 체제 정보(name, version)
browser: 브라우저 정보(name, version, major)
device: 장치 정보(model, type, vendor)
```

<br />

```javascript
import { useState, useEffect } from 'react'
import UAParser from 'ua-parser-js'

interface IUserAgentInfo {
  os: UAParser.IOS
  browser: UAParser.IBrowser
  device: UAParser.IDevice
}

function getUserAgentInfo(uaString: string): IUserAgentInfo {
  const parser = new UAParser()
  parser.setUA(uaString)

  return {
    os: parser.getOS(),
    browser: parser.getBrowser(),
    device: parser.getDevice(),
  }
}

function useUserAgent(): IUserAgentInfo {
  const [userAgentInfo, setUserAgentInfo] = useState<IUserAgentInfo>({
    os: { name: '', version: '' },
    browser: { name: '', version: '', major: '' },
    device: { model: '', type: '', vendor: '' },
  })

  useEffect(() => {
    const userAgentData = getUserAgentInfo(window.navigator.userAgent)
    setUserAgentInfo(userAgentData)
  }, [])

  return userAgentInfo
}

export default useUserAgent
```

<br />

사용예제:

```javascript
import useUserAgent from '@/hooks/useUserAgent'

const { device } = useUserAgent()
```
