---
title: '[ Ch # 09 ] 정확한 마우스 포지션 구하기 !'
date: 2021-02-23 18:21:13
category: 'react'
draft: false
---

### \* Finding the correct mouse position !

<BR />

DOM 요소에 접근하여 CSS HOVER Effect 또는 애니매이션 요소를 가미하여 보통 <BR />
마우스접근 이벤트를 발생시키지만 개발을 하다 보면 `정확한` 마우스 위치를 알아야하는 경우가 필요하다. <BR />

그것을 알아보자 !

```tsx
// mousePosition.tsx
import React, { useState, useEffect } from 'react'

export default element => {
  const [mousePostion, setMousePosition] = useState(null)

  // 여기서 useLayOutEffect를 생각해볼 수 있겠는데.. node element 요소로 인한 이벤트 케치이므로
  // useEffect가 더 어울릴거 같다.
  useEffect(() => {
    const positonHandler = e => {
      const mousePoint = {
        x: e.pageX,
        y: e.pageY,
        // PAGE X,Y 로 해당 돔요소를 지정하여 위치값을 가져오려고 하는데
        // 다르게 Client X,Y 를 통한 뷰의 위치값으로 응용 가능하다.
      }
      setMousePosition(mousePoint)
    }
    element && element.addEventListener('mousemove', positonHandler)
    // mousemove api 를 add 해주고 그 행위가 발생할때마다 positonHandler 실행
    return () => {
      element && element.removeEventListener('mousemove', positonHandler)
      // 해당 컴포넌트를 빠져 나갈때 remove api
    }
  }, [element])

  return mousePostion
  // 결국은 해당 위치값을 리턴
}
```

```tsx
// index.tsx
import React from 'react'
import useMouseChecker from './mousePosition'
import 'main.scss'

const Main = function() {
  const [element, setElement] = useState(null)
  const [colorChange, setColorChange] = useState(false)

  const getElement = targetElement => {
    setElement(targetElement)
  }

  const position = useMouseChecker(element)

  useEffect(() => {
    if (
      position &&
      position.x > 100 &&
      position.x < 300 &&
      position.y > 100 &&
      position.y < 300
    ) {
      setColorChange(true)
    } else {
      setColorChange(false)
    }
  }, [position])

  return (
    <div
      ref={getElement}
      className={colorChange ? 'mainContainer orangeBg' : 'mainContainer'}
    >
      {position && `x: ${position.x} y: ${position.y}`}
    </div>
  )
}
```

```scss
* {
    padding:0,
    margin:0
}
.mainContainer {
  width: 200px;
  height: 200px;
  border: 2px solid darkGray;

  &.orangeBg {
    background-color: orange;
  }
}
```
