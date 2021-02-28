---
title: '[ Ch # 10 ] 드롭다운 메뉴 구현하기 !'
date: 2021-02-26 18:21:13
category: 'react'
draft: false
---

### \* Dropdown that disappears when you click outside !

<BR />

어디에서나 쓰이는 드롭다운 컴포넌트를 공통 컴포넌트로 커스터마이징 해보자.

```tsx
// dropdown.tsx
import React, { useEffect, useState, useRef } from 'react'

type DropdownProps = {
  value: string
  options: Array<any>
  placeholder: string
  dispatch: any
}

const Dropdown = (props: DropdownProps) => {
  const { value, options, placeholder, dispatch } = props

  const node = useRef()

  const [open, setOpen] = useState<boolean>(false)

  const handleClick = e => {
    if (node.current.contains(e.target)) {
      // inside click
      return
    }
    // outside click
    setOpen(false)
  }
}
export default Dropdown
```
