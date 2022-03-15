---
title: '@react-testing-library'
date: 2022-01-19 16:21:13
category: 'react'
draft: false
---

## \* React Testing Library(RTL)

<b>RTL</b>은 Behavior Driven Test(행위 주도 테스트) 방법론이 떠오르며, 주목 받기 시작한 테스팅 라이브리러이다.<br />
기존의 TDD 구현주도 테스트는 주로 애플리케이션이 어떻게 작동하는지에 초점을 두어 테스트를 하는 반면 <b>BDD</b>는 사용자가 <br />
애플리케이션을 이용하는 관점에서 사용자의 <b>실제 경험 위주</b>로 테스트를 작성한다.<br /><br />

RTL은 `<jsdom>`이라는 라이브러리를 통해 실제 부라우저 DOM을 기준으로 테스트를 작성하게 된다. <br />
따라서 어떤 React 컴포넌트를 사용하는지는 의미가 없으며, 결국 사용자의 브라우저에서 랜더링하는 실제 HTML 마크업의 모습이 어떤지에 대해 테스트하기 용이 <br /><br />

### RTL 주요 API

1. `render()` : DOM에 컴포넌트를 랜더링 <br />
   /인자로 랜더할 React Component를 넘기며, RTL에서 제공하는 모든 쿼리 함수와 기타 유틸 함수를 담고 있는 객체를 리턴한다./<br />

```JSX
import { render, fireEvent } from '@testing-library/react';

const { getByText, getByLabelText, getByPlaceholderText } = render(<TestComponent />)
```

<br />

2. `fireEvent()` : 특정 이벤트를 발생시켜주는 객체 <br />
   /쿼리 함수로 선택된 영역을 대상으로 특정 이벤트를 발생시키기 위한 이벤트 함수들을 담고 있다./<br /><br />

---

api Cheatsheet: https://testing-library.com/docs/dom-testing-library/cheatsheet

---

### RTL 테스트 SAMPLE

```JSX
// static 컴포넌트
import React from 'react';

function ErrorComponent({ path }) {
  return (
    <div>
      <h1>에러 페이지</h1>
      <p>해당 페이지({path})를 찾을 수 없습니다.</p>
      <img
        alt="error page"
        src="error.com"
      />
    </div>
  )
}
```

```JSX
// static 컴포넌트 Test
import React from 'react';
import { render } from '@testing-library/react';
import ErrorComponent from 'ErrorComponent';

describe('<ErrorComponent />', () => {
  it('renders header title', () => {
    const { getByText } = render(<ErrorComponent path="/err" />);
    const header = getByText("에러 페이지");

    expect(header).toBeInTheDocument();
  })

  it('renders paragraph', () => {
    const { getByText } = render(<ErrorComponent path="/err" />);

    const paragraph = getByText(/^해당 페이지/);
    expect(paragraph).toHaveTextContent("해당 페이지(/err)를 찾을 수 없습니다.");
  });

  it('renders image', () => {

  });
});

```
