---
title: '[ Ch # 11 ] 스크롤 인디케이터 구현하기 !'
date: 2021-03-10 18:21:13
category: 'react'
draft: false
---

### \* Scroll Indicator !

/\* Scroll Indicator? Client에서 Scroll Event를 발생시킬 때 현재 스크롤된 영역이 전체 컨텐츠 영역의 몇 % 진행되었는가를 계산하여 눈으로 보여준다.
Scroll Event를 발생시키면 Event Target - scrollingElement 프로퍼티를 활용해 현재 스크롤 상태와 관련된 정보를 얻을 수 있다.

```tsx
//index.tsx
import React, { useEffect, useRef } from 'react'
import { scrollIndicatorFn } from './indicator'
import './styles.css'

export default function App() {
  const indicatorRef = useRef<null>()
  // useRef를 통한 element 와 event target 접근
  useEffect(() => {
    const scrollIndicator = indicatorRef.current
    // useEffect를 이용하여 첫번째 렌더링이 된 직후의 current 값을 가져와야
    // indicator가 그려지기전 ref 값을 가져오려는 것을 방지.

    window.addEventListener('scroll', e => {
      scrollIndicatorFn(e, scrollIndicator)
      // 공통함수에 event와 indicator 정보를 넘겨준다.
    })

    return () => {
      window.removeEventListener('scroll', e => {
        scrollIndicatorFn(e, scrollIndicator)
      })
      // 컴포넌트를 벗어나게되면 이벤트 삭제
    }
  }, [])

  return (
    <div className="App">
      <header>
        <h2>스크롤 인디케이터 !</h2>
        <div className="indicator" ref={indicatorRef} />
      </header>
      <section>컨텐츠내용...</section>
    </div>
  )
}
```

```tsx
// indicator.tsx (범용성을 위한 공통함수로 export)
export const scrollIndicatorFn = (e, scrollIndicator) => {
  const { scrollTop, scrollHeight, clientHeight } = e.target.scrollingElement
  // scrollTop : 현재 스크롤된 부분의 맨 위의 높이
  // scrollHeight : 문서의 총 높이 (= 스크롤의 총 높이)
  // => 이부분이 어려울수 있겠는데 ?쉽게 생각하면 눈에 보이는 높이에서 스크롤한 높이를 더해준것과 같다.
  // clientHeight : 웹 브라우저 창의 높이
  const contentHeight = scrollHeight - clientHeight
  // contentHeight : 전체 총 높이에서 클라이언트 높이를 뺀 것
  const percentage = (scrollTop / contentHeight) * 100
  // 전체 높이에서 스크롤이 진행된 비율
  scrollIndicator.style.transform = `translateX(-${100 - percentage}%)`
  // indicator 의 css를 살펴보면 init으로 transform: translateX(-100%);를 부여 합니다.
  // 스크롤과 컨텐츠의 진행 상황이 0인 경우 client view에서 벗어나기 위해서입니다.
  // 스크롤이 진행된다면 tranlatx의 수치를 증가시켜 끝부분부터 보이게 하는 fake 모션입니다.
  scrollIndicator.style.transition = `transform 0.5 ease-in-out`
}
```

```css
body {
  margin: 0px;
}

header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: fixed;
  width: 100%;
  height: 80px;
  text-align: center;
  border-bottom: 2px solid peachpuff;
  background-color: #fff;
  box-sizing: border-box;
}

.indicator {
  position: absolute;
  bottom: 1px;
  left: 0;
  width: 100%;
  height: 4px;
  background-color: sandybrown;
  transform: translateX(-100%);
}

section {
  padding: 80px 10px 0px 10px;
}
```

`youtube:https://youtu.be/2eHe6IrTFcM`
