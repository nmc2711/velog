---
title: 'React query'
date: 2022-03-18 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">React-query란?</h3>

React를 위한 데이터 동기화 라이브러리 <br />
react-query는 서버의 값을 클라이언트에 가져오거나, 캐싱, 값 업데이트, 에러핸들링 등 비동기 과정을 더욱 편하게 하는데 사용된다.<br />

<h3 style="color:#0230">React-query를 사용하는 이유는 뭘까?</h3>

위와같은 정형적인 react-query의 소개를 바탕으로 사용하는 경우가 크겠지만, 실제 업무에서 프로젝트가 커지게 되면<br />
client의 상태와 server로 부터 받아 가공되어 저장되는 data들의 상태를 관리하는데 큰 컴플렉스가 일어난다.<br />
그것을 효율적이게 관리하기 위함이다.<br />

<h3 style="color:#0230">React-query의 장점</h3>
- 캐싱
- get을 한 데이터에 대해 update를 하면 자동으로 get을 수행하는 automatic fetcher
- 오래된 데이터라고 판단시 다시 get( invalidateQueries )
-

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
