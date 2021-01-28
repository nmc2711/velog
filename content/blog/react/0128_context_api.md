---
title: '[ Ch # 02 ] Context Api 구조설계 '
date: 2021-01-28 18:00:00
category: 'react'
draft: false
---

## React 전역 상태관리, Context Api 구조 설계

> React 내에서 대표적인 상태관리 라이브러리는 Redux라고 할수있다.
> 체계적인 시스템으로 많은 사람들이 사용하지만, 2021년 현 시점부터
> React 내장 useContext가 점유율이 가장 높다고 한다고 합니다.

함 짜봅시당 !!!!!!!

```js
// context/global_ctx.tsx
import React, { useContext } from 'react'
import { GlobalProvider } from 'context/global_ctx'

ReactDOM.render(
  <React.StrictMode>
    <GlobalProvider>
      <App />
    </GlobalProvider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

```js
// Index.tsx
import React, { useContext } from 'react'
import { GlobalProvider } from 'context/global_ctx'

ReactDOM.render(
  <React.StrictMode>
    <GlobalProvider>
      <App />
    </GlobalProvider>
  </React.StrictMode>,
  document.getElementById('root')
)
```
