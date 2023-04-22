---
title: useScreenSize
date: 2023-04-22 17:04:72
category: hooks
thumbnail: { thumbnailSrc }
draft: false
---

`useScreenSize` 커스텀 훅은 현재 뷰포트 크기와 기기 유형을 React에서 관리하고 감지하는 데 도움이 되도록 구현하였습니다. <br />
사용자 브레이크포인트를 지정하거나 기본값을 사용하여 디바이스 플래그를 만들어 불리언으로 반환하여 사용할 수도 있습니다.<br />

```
1. 현재 뷰포트 너비(currentWidth)와 높이(currentHeight)를 State로 담아둡니다.

2. useEffect를 사용하여 컴포넌트가 마운트 될 때 브라우저 창 크기가 변경될 때마다 호출되는 handleChangeSize 이벤트 리스너로 추가합니다.

handleChangeSize 함수는 getScreenSize 함수를 호출하여 현재 뷰포트 크기를 가져옵니다.
window.innerWidth 및 window.innerHeight를 사용하여 창의 너비와 높이를 가져온 후, useState를 통해 상태를 업데이트합니다.

3. 언마운트 될 때 추가한 이벤트 리스너를 제거합니다.

4. 뷰포트 너비(currentWidth)와 브레이크포인트를 비교합니다.

5. isLargeDesktop, isDesktop, isTablet, isMobile을 반환합니다.

```

```javascript
import { useEffect, useState } from 'react'
type Sizes = {
  medium: number
  small: number
  large: number
}

const defaultSizes: Sizes = {
  medium: 768,
  small: 480,
  large: 1024,
}

const isClientSide = typeof window === 'object'

const getScreenSize = () => {
  const width =
    window.innerWidth ||
    document.documentElement.clientWidth ||
    document.body.clientWidth

  const height =
    window.innerHeight ||
    document.documentElement.clientHeight ||
    document.body.clientHeight

  return { width, height }
}

const useScreenSize = (breakpoints = defaultSizes) => {
  const [screenSize, setScreenSize] = useState({ width: 0, height: 0 })

  useEffect(() => {
    if (!isClientSide) return
    const handleChangeSize = () => {
      setScreenSize(getScreenSize())
    }

    window.addEventListener('resize', handleChangeSize)
    handleChangeSize()

    return () => window.removeEventListener('resize', handleChangeSize)
  }, [])

  const { width: currentWidth, height: currentHeight } = screenSize

  const isLargeDesktop = currentWidth > breakpoints.large
  const isDesktop =
    currentWidth > breakpoints.medium && currentWidth <= breakpoints.large
  const isTablet =
    currentWidth > breakpoints.small && currentWidth <= breakpoints.medium
  const isMobile = currentWidth <= breakpoints.small

  return {
    currentWidth,
    currentHeight,
    isLargeDesktop,
    isDesktop,
    isTablet,
    isMobile,
  }
}

export default useScreenSize
```

<br />

사용예제:

```javascript
import useScreenSize from '@/hooks/useScreenSize'

const { isMobile } = useScreenSize()

if (isMobile) {
  // ..
}
```
