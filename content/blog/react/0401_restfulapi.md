---
title: '[ Ch # 13 ] Restful Api !'
date: 2021-04-01 18:21:13
category: 'react'
draft: false
---

## Client Side Restful Api Fetch 구조설계

npm install node-fetch
=> window.fetchNode.js 를 가져 오는 경량 모듈

// using fetch에 관한 기본지식 https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch

// 먼저, 가장 필수는 BackEnd 파트와의 협의 입니다. 각 회사별 시스템이 다를테니 구조는 항상 유연합니다.<br />
// 예를 들어 BackEnd 파트 와의 협의를 마치고 정의된 API 명세서를 확인해 보겠습니다.<br />
// API Domain: 'https://www.example-api.com/'<br />
// API명 : '메인페이지 컨텐츠 리스트'<br />
// API URL : '/main/list'<br />
// API METHOD : 'GET'<br />
// 항상 json 타입의 문자열로 {header:{exampleToken:"exampleToken"},method:'get'}} 해더와 method 타입의 option을 요구합니다.<br />
// 파라미터로 필수값 number 타입의 page 와 string 타입의 category를 요구합니다.<br />
// status로 성공한다면 200, 실패시 404 를 반환<br />
// 성공한다면 반환값으로 result 와 data json을 반환해줍니다.

```ts
// common/api.ts
// 먼저 공통으로 사용될 api location 을 정의합니다.
const API_LOCATION = 'https://www.example-api.com'

enum Method {
  GET = 'get',
  POST = 'post',
  DELETE = 'delete',
}

enum HttpStatus {
  OK = 200,
  NotFound = 404,
}
type responseType = {
  result: string
  data: any
}

type dataType = {
  [key: string]: any
}

const ajax = async (method: string, path: string, data?: dataType) => {
  // 결국 저희는 'https://www.example-api.com/main/list' 까지 API URL 이며
  // 파라미터 값에 따라 API_LOCATION + ? + page=1&category="cartoon"
  // 즉,호출해야 하는 url은 'https://www.example-api.com/main/list?page=1&category=cartoon' 입니다.
  // 항상 전달해줘야하는 option 값은 {headers:'exampleToken',method:method}

  const headers = {
    exampleToken: 'exampleToken', // cookie 등등 Token 을 저장하고 정의할 방법은 미리  세팅되어 가져올수있어야합니다.
  }
  const option = { method }
  option['headers'] = headers
  // backEnd에서 항상 요구하는 method와 header키 안에 exampleToken 값을 저장합니다.

  let url: string = (() => {
    return API_LOCATION + path
    // 호출할 url을 정의합니다. 'https://www.example-api.com/main/list'
  })()

  if (data !== undefined) {
    if (method === Method.GET) {
      // method타입이 get이며 요구하는 parameter 가 1개 혹은 2개이상일때의 url을 유연하게 정의합니다.
      let queryString = '?'
      Object.keys(data).forEach((key, i, self) => {
        if (self.length === i + 1) {
          queryString += `${key}=${data[key]}`
        } else {
          queryString += `${key}=${data[key]}&`
        }
      })
      url += queryString
      // get일 경우
    }
  }

  return await fetch(url, option).then(res => {
    // url ,option api 통신할 주소와 필요로 하는 option의 json을 전달하며 fetch를 한다.
    if (res.status === HttpStatus.OK) {
      // 성공시 res를 json 타입으로 파싱후 사용
      return res.json()
    }
    return {
      result: 'fail',
      data: null,
    }
  })
}

// export 함수로 뽑아 각 api 요소별 재사용성을 지향합니다
export async function mainList(data: {
  page: number
  category: string
}): Promise<responseType> {
  return await ajax(Method.GET, '/main/list')
  // Fetch 함수에 method 타입과 url 넣어 통신
}
```

이제 정의된 restful api 와 공통 export 함수를 통한 api통신을 해보겠습니다.
//src/main/index.tsx

```tsx
import React, { useState, useContext, useEffect, useCallback } from 'react'
import { mainList } from 'common/api'
// mainList export함수 import

const Main = () => {
  const [list, setList] = useState<Array<any>>([])

  const fetchMainList = useCallback(() => {
    mainList({ page: 1, category: 'cartoon' }).then(res => {
      const { data, result } = res
      if (result === 'success') {
        setList(data.list)
      } else {
        console.log(result)
      }
    })
  }, [])

  useEffect(() => {
    fetchMainList()
  }, [])

  return (
    <React.Fragment>
      {list &&
        list.map((item, idx) => {
          return <div>{item.title}</div>
        })}
    </React.Fragment>
  )
}
export default Main
```
