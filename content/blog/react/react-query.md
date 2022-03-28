---
title: 'React query'
date: 2022-03-18 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">React-query란?</h3>

React를 위한 데이터 동기화 라이브러리 <br />
전역 상태를 건드리지 않고 어플리케이션에서 데이터를 가져오고 업데이트할 수 있다. <br />

```javascript
// useQuery basic pattern

function func() {
  const { isLoading, isError, data, error } = useQuery(['state', text], () =>
    doSomething(text)
  )

  if (isLoading) {
    return <div>loading...</div>
  }
  if (isError) {
    return <div>Error : {error.message}</div>
  }

  return (
    <div>
      {data.map(some => (
        <p key={some.id}>{some.text}</p>
      ))}
    </div>
  )
}
```

useQuery()는 두 개의 인자를 받는데 첫 번째 인자는 unique key, 두 번째 인자는 Promise를 리턴하는 함수이다.<br />
unique key는 보통 배열의 형태로 들어가며 쿼리의 이름을 나타내는 문자와 함수의 인자로 쓰이는 값을 넣는다고 한다. 해당 key값으로 데이터를 캐싱한다.<br />
Promise를 리턴하는 함수는 말 그대로 원하는 비동기 함수를 넣어준다.<br />

간단하게 상태를 확인하고 원하는 값을 서버에서 얻어올 수 있다.

```javascript
function func() {
  const mutation = useMutation(state => axios.post("/post", text));

  return (
    <div>
      { mutation.isLoading ? (
        'Loading...'
      ) : (
        <>
          { mutation.isError ? (
            <div>Error</div>
          ) : null }
          { mutation.isSuccess ? (
            <div>Success</div>
          ) : null }

          <button
            onClick={() => { mutation.mutate("") }}
          />
            Post Request
          </button>
        </>
      )}
    </div>
  )
}
```

useQuery와 다르게 create, update, delete 하며 서버측에 데이터 변화를 발생시킨다.<br />
