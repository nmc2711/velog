---
title: 'React 18v'
date: 2022-04-06 16:21:13
category: 'react'
draft: false
---

1. useId

<br />

`useId`는 클라이언트와 서버간 hydration의 mismatch를 피하면서 유니크 아이디를 생성할 수 있는 훅 <br />
이는 주로 고유한 id가 필요한 접근성 api와 사용되는 컴포넌트에 유용할 것으로 기대된다. <br />

```javascript
import { useId } from 'react'

const PartComponent = () => {
  const id = useId();
  return (
    <div>
      {id}
      <SubComp>
    </div>
  );
}

const SubComponent = () => {
  const id  = useId();
  return (
    <div>{id}</div>
  )
}


const IdComponent = () => {
  const id = useId();
  return (
    <>
      {id}
      [1, 2, 3].map((item, idx) => {
        return (
         <PartComponent key={idx} />
        );
      })
    </>
  );
}

IdComponent: :r0:
PartComponent:r1:
SubComponent: :r2:
 .
 .
 .
```

유니크한 아이디부여 및 순서보장
