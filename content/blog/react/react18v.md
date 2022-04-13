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
느린 비동기 통신의 timeoutMs지연을 기다린 응닶값을 통해 완성된 뷰를 갱신할떄 좋을 것 같다.<br />

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

4. useSyncExternalStore

<br />

`useSyncExternalStore`는 동기적으로 스토어 업데이트를 강제해서 외부 스토어가 동시 읽기를 지원할 수 있는 새로운 훅. <br />
외부 데이터 소스에 대한 구독을 구현할 때 useEffect가 필요하지 않으며 react 외부 상태와 통합되는 모든 라이브러리에 권장된다. <br />

```javascript
const firstExternalStore = useSyncExternalStore(
  store.getSnapshot(),
  () => store.getSnapshot().check
)
```

<br />

`useSyncExternalStore`는 두개의 함수를 인자로 받는다. <br />
subscribes는 등록할 콜백 함수, getSnapshot 마지막 이후로 subscribe 중인 값이 렌더링 된 이후 변경되었는지, 문자열이나 숫자처럼 <br />
immutable한 값인지, 혹은 캐시나 메모된 객체인지 확인하는데 사용된다. 이후, 훅에 의해 immutable한 값이 반환된다. <br />

```javascript
import { useSyncExternalStore } from 'react'

const useStore = (store, selector) => {
  return useSyncExternalStore(
    store.subscribe,
    useCallback(() => selector(store.getState(), [store, selector]))
  )
}
```

`startTransition`은 startTransition에 래핑된 업데이트는 긴급하지 않은 것으로 처리되며 클릭이나 키 누르기와 같은 더 긴급한 업데이트가 들어오면 중단된다. <br />
트랜지션이 사용자에 의해 중단되면 (예로 한 줄에 여러 문자를 입력), React는 완료되지 않은 오래된 렌더링 작업을 버리고 최신 업데이트만 렌더링한다.<br />

useTransition: 보류 중인 상태(the pending state)를 추적하는 값을 포함한 트랜지션 시작 훅<br />
startTransition: 훅을 사용할 수 없을 때 트랜지션을 시작하는 메서드<br />
트랜지션은 동시성 렌더링을 선택하여 업데이트를 중단할 수 있다. <br />
콘텐츠가 다시 중단된 경우(re-suspends)라면, 트랜지션은 백그라운드에서 트랜지션 콘텐츠가 렌더링하는 동안 현재<br /> 콘텐츠를 계속 표시하도록 React에 지시한다.<br />

```javascript
import { startTransition } from 'react'

// 긴급: 어떤 입력인지 표시
setInputValue(input)

// 내부의 모든 상태 업데이트를 transitions으로 표시
startTransition(() => {
  // Transition: 결과를 표시
  setSearchQuery(input)
})
```

<br />

`useInsertionEffect`는 css-in-js 라이브러리가 렌더링 도중에 스타일을 삽입할 때 성능 문제를 해결할 수 있는 새로운 훅이다. <br />css-in0js 라이브러리를 사용하지 않는다면 사용할 필요가 없다. 이 훅은 dom이 한번 mutate된 이후에 실행되지만, <br />layout effect가 일어나기전에 새 레이아웃을 한번 읽는다. 이는 리액트 17 이하 버전에 있는 문제를 해결할 수 있으며, <br />리액트 18에서는 나아가 concurrent 렌더링 중에 브라우저에 리액트가 값을 반환하므로, 레이아웃을 한번더 계산할 수 있는 기회가 생겨 매우 중요하다.<br />

어떻게 보면 useLayoutEffect와 비슷한데, 차이가 있다면 DOM 노드에 대한 참조에 엑세스 할 수 있다는 것이다.<br />

클라이언트 사이드에서 style 태그를 생성해서 삽입할 때는 성능 이슈에 대해 민감하게 살펴보아야 한다.<br /> CSS 규칙을 추가하고 삭제한다면 이미 존재하는 모든 노드에 새로운 규칙을 적용하는 것이다. <br />이는 최적의 방법이 아니므로 많은 문제가 존재한다.
<br />
이를 피할 수 있는 방법은 타이밍이다. 리액트가 DOM을 변환한경우, 레이아웃에서 무언가를 읽기전 (clientWidth와 <br />같이) 또는 페인트를 위해 브라우저에 값을 전달하기 전에 DOM에 대한 다른 변경과 동일한 타이밍에 작업을 하면 된다.

```javascript
function useCSS(rule) {
  useInsertionEffect(() => {
    if (!isInserted.has(rule)) {
      isInserted.add(rule)
      document.head.appendChild(getStyleForRule(rule))
    }
  })
  return rule
}
function Component() {
  let className = useCSS(rule)
  return <div className={className} />
}
```
