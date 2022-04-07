---
title: 'React 18v'
date: 2022-04-06 16:21:13
category: 'react'
draft: false
---

최근 릴리즈된 react 18v 핵심 변화요소들을 알아보자. <br />

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
      <SubComponent />
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

// IdComponent:r0
// PartComponent:r1
// SubComponent:r2
// PartComponent2:r3
// SubComponent2:r4
// ......
// 유니크한 아이디부여 및 순서보장
```

<br />

2. startTransition, useTransition

<br />
이 두 메소드를 사용하면 일부 상태 업데이트를 긴급하지 않은 것(not urgent)로 표시할 수 있다. <br />
이것으로 표시되지 않은 상태 업데이트는 긴급한 것으로 간주된다. 긴급한 상태 업데이트(input text 등)가 긴급하지 <br />
않은 상태 업데이트(검색 결과 목록 렌더링)을 중단할 수 있다. <br />

상태 업데이트를 긴급한것과 긴급하지 않은 것으로 나누어 개발자에게 렌더링 성능을 튜닝하는데 많은 자유를 주었다고 볼 수 있다. <br />

```javascript
function Book() {
  const [list, setList] = useState([{ ... }]);
  const [startTransition, isPending] = useTransition({ timeoutMs: 3000 });

  function onDelay() {
    startTransition(() => {
      const nextBook = getId((list.bookId));
      setList(apiCall.get(nextBook));
    });
  }

  return (
    <>
      <button onClick={onDelay} diabled={isPending}>다음 책 보기</button>
      {isPending ? <p>'로딩중...'</p> : <BookView list={list} />}
    </>
  );
}
export default Book;

// 다음 책 보기 버튼을 누르면 isPending이 true로 바뀌면서 로딩중 p태그가 나타난다.
// 버튼이 비활성화되고 3초가 흐르면 startTransition 콜백함수가 실행되고 isPending이 기본값으로 바뀌며
// 다음 책의 모습이 그려진다.
```

`startTransition`은 함수로, 리액트에 어떤 상태변화를 지연하고 싶은지 지정할 수 있다.<br />
`isPending`은 진행 여부로, 트랜지션이 진행중인지 알 수 있다.<br />
`timeoutMs`로 최대 3초간 이전 상태를 유지한다.<br />

<br />

3. useDeferredValue

<br />
`useDeferredValue`를 사용하면, 트리에서 급하지 않은 부분의 리랜더링을 지연할 수 있다.<br />
최대 timeoutMs 동안 지연된값의 지연된 결과를 반환한다 .<br />
느린 비동기 통신의 timeoutMs지연을 기다린 응닶값을 통해 완성된 뷰를 갱신할떄 좋을것같다.<br />

```javascript
function ProductList({ products }) {
  const deferredProducts = useDeferredValue(products, { timeoutMs: 2000 })
  return (
    <ul>
      {deferredProducts.map(product => (
        <li>{product}</li>
      ))}
    </ul>
  )
}
```
