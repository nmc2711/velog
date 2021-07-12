---
title: '리액트에 녹인 공통 useDebounce 커스텀 훅 구현하기!'
date: 2021-07-12 18:21:13
category: 'react'
draft: false
---

## React로 Usedebounce Customhooks 구현해보기!

먼저, Debounce 는 여러번 발생하는 이벤트에서, 가장 마지막 이벤트 만을 실행 되도록 만드는 개념이다. <br />
입력이 끝날때, 가장 마지막 이벤트만을 실행하여, 성능성 유리함을 가져올 수 있다. <br />
Throttle 와 다른점은, 마지막 이벤트에서 일정 시간동안 이벤트가 발생한다면, 또 일정 시간을 기다린다는 점이다. <br />

```tsx
// useDebounce.tsx

import { useEffect, useState } from 'react'

function useDebounce<T>(value: T, delay?: number): T {
  const [debounceValue, setDebounceValue] = useState<T>(value)

  useEffect(() => {
    // 파라미터로 넘겨받은 value를 타이머로 랩핑을 하여 state 업데이트에 시간차를 준다.
    // 최종적으로 시간차를 준 state를 sideEffect의 depth로 활용하여 debounce 구현
    const timer = setTimeout(() => {
      setDebounceValue(value)
    }, delay || 500)

    return () => clearTimeout(timer)
  }, [value, delay])

  return debounceValue
}

export default useDebounce
```

```tsx
// app.tsx
import { ChangeEvent, useEffect, useState } from 'react'

import useDebounce from './useDebounce'

export default function App() {
  const [value, setValue] = useState<string>('')
  const debouncedValue = useDebounce<string>(value, 500)

  const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    setValue(event.target.value)
  }

  useEffect(() => {
    // api fetch나 비동기 작업에 수행 해당 side effect안에 작성
    // debounce가 걸린 debouncedValue 변화에 따라 함수실행
  }, [debouncedValue])

  return (
    <div>
      <p>스테이트 변화: {value}</p>
      <p>디바운스 스테이트 변화: {debouncedValue}</p>

      <input type="text" value={value} onChange={handleChange} />
    </div>
  )
}
```

<br />

코드첨부

https://codesandbox.io/s/usedebounce-v81gn?file=/src/App.tsx:0-670
