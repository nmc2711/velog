---
title: useDebounce
date: 2023-05-04 17:04:72
category: hooks
thumbnail: { thumbnailSrc }
draft: false
---

`useDebounce`는 입력값이 변경된 후 일정 시간이 지난 후에야 변경 사항을 적용합니다. <br />

사용자가 입력하는 동안 리소스를 많이 사용하는 작업이 지속적으로 발생하는 것을 방지하여 성능을 개선하는 데 도움이 됩니다.<br />

useDebounce는 제네릭 타입 T를 사용하여 다양한 타입의 값을 받을 수 있도록 하며, value와 delay를 인자로 받습니다.<br />

기본적으로 delay는 500ms로 설정되어 있습니다. <br />

```javascript
import { useEffect, useState } from 'react'

function useDebounce<T>(value: T, delay = 500) {
  const [debouncedValue, setDebouncedValue] = useState < T > value

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay)

    return () => {
      clearTimeout(timer)
    }
  }, [value, delay])

  return debouncedValue
}

export default useDebounce
```
