---
title: 'React Portals'
date: 2022-03-15 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">Portal 이란?</h3>

DOM 계층 구조 바깥에 있는 DOM 노드로 자식을 렌더링하는 최고의 방법

```javascript
// basic pattern
ReactDOM.createPortal(child, container)

// 첫 번째 인수 (child)는 요소, 문자열 또는 조각과 같은 렌더링 가능한 React child 이다.
// 두 번째 인수 (container)는 DOM 요소이다.
```

<h3 style="color:#0230">왜 사용할까?</h3>

일반적으로 react는 부모 컴포넌트가 렌더링되면 자식 컴포넌트가 렌더링되는 tree 구조를 가지고 있다. <br />

하지만 때때로 이런 tree구조가 불편함을 가져다주기도 한다. 분명 부모-자식 관계를 가지고 있지만 독립적인 위치에서 렌더링을 하면 훨씬 편리한 경우가 있다.<br />

대표적인 예로 modal은 부모 컴포넌트의 스타일링 속성에 제약을 받아 z-index 등으로 번거로운 후처리를 해줘야한다.<br />

이러한 상황에서 portal을 통해 독립적인 구조와 부모-자식 관계를 동시에 유지할 수 있다면, z-index 등 부모 컴포넌트의 제약에서 벗어날 수 있다. <br />

<h3 style="color:#0230">사용패턴</h3>

```html
<body>
  <div id="root"></div>
  <div id="modal"></div>
</body>
```

```javascript
//portal.js
import reactDom from 'react-dom'

const Portal = ({ children }) => {
  const el = document.getElementById('modal')
  return reactDom.createPortal(children, el)
}

export default Portal
```

```javascript
// modal.js
import Portal from './portal'
import styled from 'styled-components'

const Modal = ({ onClose }) => {
  return (
    <Portal>
      <Background>
        <Content>show modal</Content>
      </Background>
    </Portal>
  )
}

const Background = styled.div`
  height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  left: 0;
  top: 0;
  text-align: center;
`

const Content = styled.div`
  height: 100%;
  width: 950px;
  margin-top: 70px;
  position: relative;
  overflow: scroll;
  background: #141414;
`
```

```javascript
// view modal

import styled from 'styled-components'
import Modal from './modal'

const MyModal = props => {
  const [open, setIsOpen] = useState(false)

  const handleModal = () => {
    setIsOpen(!open)
  }

  return (
    <>
      <Container>
        <button onClick={handleModal}>show modal</button>
        {modalOn && <Modal onClose={handleModal} />}
      </Container>
    </>
  )
}
export default MyModal
```
