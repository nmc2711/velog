---
title: 'React-use-measure'
date: 2022-01-15 16:21:13
category: 'development'
draft: false
---

웹과 달리 모바일 환경의 웹에서 터치 혹은 드레그의 좌표를 받기가 쉽지않다. <br />

라이브러리를 서치중 해당하는 좋은 라이브러리를 소개한다. <br />

```javascript
import useMeasure from 'react-use-measure'

function App() {
  const [ref, bounds] = useMeasure()

  // consider that knowing bounds is only possible *after* the view renders
  // so you'll get zero values on the first run and be informed later

  return <div ref={ref} />
}
```

https://codesandbox.io/s/musing-kare-4fblz?file=/src/index.tsx:530-531
