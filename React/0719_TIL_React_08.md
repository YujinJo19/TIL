#### 목차

- [React](#react)
  * [1. Redux](#1-redux)
    + [1-1. 기존 Redux를 이용한 상태 관리](#1-1-기존-redux를-이용한-상태-관리)
    + [1-2. 기존 Redux의 비동기 처리](#1-2-기존-redux의-비동기-처리)
    + [1-3. 기존 Redux의 문제점](#1-3-기존-redux의-문제점)
  * [2. SWR](#2-swr)
    + [2-1. SWR이란](#2-1-swr이란)
    + [2-3. SWR의 장점](#2-3-swr의-장점)
    + [2-4. Redux의 대체](#2-4-redux의-대체)
  * [출처](#출처)



# React

## 1. Redux 

### 1-1. 기존 Redux를 이용한 상태 관리

- 리듀서로 정의한 초기 상태와 변이 방법

```javascript
function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```

- 컴포넌트

```javascript
import {useSelector} from 'react-redux'

function Counter(){
    const data = useSelector(state => state)
    return <div>count: {data}</div>
}
```

- state의 값 변경

```javascript
store.dispatch({ type: 'INCREMENT' })
// 1
store.dispatch({ type: 'INCREMENT' })
// 2
store.dispatch({ type: 'DECREMENT' })
// 1
```



### 1-2. 기존 Redux의 비동기 처리

- redux-thunk

```javascript
function asyncAction(){
  return (dispatch) => {
    fetchHowTo('/api/how-to').then(howTo => {
      dispatch({type: howTo})
    })
  }
}
store.dispatch(asyncAction())
```

- redux-saga

```javascript
function* sagaHowto() {
   const howTo = yield call(fetchHowTo, '/api/how-to')
   yield put({type: howTo})
}
function* mySaga() {
  yield takeEvery("HOW_TO", sagaHowto);
}
store.dispatch({type: 'HOW_TO'})
```



### 1-3. 기존 Redux의 문제점

- 상태와 변이 방법을 정의하기 위한 리듀서와 액션의 코드량이 많음
- 효과적으로 상태를 초기화하기 위한 고민 필요
- 지속적으로 로컬 스토어 상태를 원격 서버 상태와 동기화해야 하는 추가 작업 필요



## 2. SWR

### 2-1. SWR이란

- Next.js로 유명한 vercel에서 만든 원격 데이터 fetch를 위한 커스텀 훅 npm 모듈
- 원격서버의 상태를 가져와서 리액트 컴포넌트에 꽂아주는 역할

```javascript
import useSWR from 'swr'

function Point(){
  const {data, error} = useSWR('/api/points', url => {
    return fetch(url).then(res => res.json())
  })
  
  if(error){
    return <div>failed to load</div>
  }
  if(!data){
    return <div>Loading..</div>
  }
  return <div>{data}</div>
}
```

- `useSWR`은 첫번째 인자로 원격상태에 대한 key를, 두번째 인자로 fetch 함수를 받음
- 첫번째 인자는 두번째 fetch 함수의 첫번째 인자로 전달됨
- fetch  함수가 데이터를 로드하면 해당 응답이 `data`로 세팅되고 오류 발생 시 해당 오류가 `error`에 세팅됨
- 컴포넌트에서는 `data`와 `error` 상태에 따라 알맞게 결과를 렌더링 해주면 됨



### 2-3. SWR의 장점

- `useSWR`은 한번 fetch한 원격 상태의 데이터를 내부적으로 캐시하고 다른 컴포넌트에서 동일한 상태를 사용하고자 할 경우 이전에 캐시했던 상태 그대로 리턴해주기 때문에 서로 다른 컴포넌트가 동일한 상태를 공유할 수 있음

- SWR은 원격상태와 로컬상태를 하나로 통합함
  - 데이터를 마치 원격상태와 연결된 데이터 스트림으로서 바라볼 수 있도록 데이터  fetching 단계를 추상화
- 내부적으로 적절한 타이밍에 지속적으로 데이터를 폴링(polling)하기 때문
  - 폴링 : 일정한 주기를 가지고 서버와 응답을 주고 받는 방식
  - 주기가 짧으면 서버의 성능에 부담이 가고 길어지면 실시간성이 떨어짐



### 2-4. Redux의 대체

```javascript
// usePoints.js
import useSWR from 'swr'

export default () => {
  const {data, error} = useSWR('/api/points', url => {
    return fetch(url).then(res => res.json())
  })
  return {data, error}
}

// useUsers.js
import useSWR from 'swr'

export default () => {
  const {data, error} = useSWR('/api/users', url => {
    return fetch(url).then(res => res.json())
  })
  return {data, error}
}
```

- 개별 상태들을 정의하여 여러 컴포넌트들에서 필요한 상태를 가져다 사용할 수 있음
- 수정 즉시 화면에 변경된 데이터가 보여할 경우에는 `mutate`함수를 이용할 수 있음
  - `mutate`함수가 호출되면 해당 상태를 즉시 다시 fetch하고 데이터를 갱신함

```javascript
import useSWR from 'swr'

function UserInfo(){
  const {data, error, mutate} = useSWR('/api/users', url => {
    return fetch(url).then(res => res.json())
  })
  
  const handleChange = async (user) => {
    await updateUser(user)
    mutate()
  }  

  return <div>~생략~</div>
}
```

- `mutate`함수를 사용할 때 데이터 fetch없이 로컬의 캐시되어있던 상태만 갱신하는 것도 가능

```javascript
 const handleChange = async (user) => {
    await updateUser(user)
    mutate(user, false) 
    // 첫번재 인자로 갱신할 데이터, 두번째 인자로 데이터 fetch 여부를 인자로 받습니다.
  } 
```

- 기본적으로는 원격서버의 상태를 fetch 하는 데 적합한 도구지만 로컬의 상태만을 필요로 하는 경우에도 충분히 사용할 수 있음
- 필요에 따라 `window`, `sessionStorage`, `localStorage`, 클로저 변수 등을 사용할 수 있음

```javascript
import useSWR from 'swr'

function useCounter(){
  const {data, mutate} = useSWR('state', () => window.count)

  return {data, mutate: (count) => {
    window.count = count
    return mutate()
  }}
}

function Counter(){
  const {data, mutate} = useCounter()
  
  const handleInc = () => mutate(data + 1)
  const handleDec = () => mutate(data - 1)

  return (
      <div>
        <span>count: {data}</span>
        <button onClick={handleInc}>inc</button>
        <button onClick={handleDec}>dec</button>
      </div>
  )
}
```





## 출처

- https://velog.io/@bisari31/Redux-Toolkit%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-TodoList-%EB%A7%8C%EB%93%A4%EA%B8%B0
- https://www.youtube.com/playlist?list=PLuHgQVnccGMB-iGMgONoRPArZfjRuRNVc
