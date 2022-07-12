#### 목차

- [React](#react)
  * [1. Redux](#1-redux)
    + [1-1. 사용해보기](#1-1-사용해보기)
  * [출처](#출처)

# React

## 1. Redux

### 1-1.  사용해보기

- `configureStore`

  ```javascript
  import { configureStore } from '@reduxjs/toolkit'
  
  export const store = configureStore({
      reducer: {},
  });
  ```

  ```javascript
  ReactDOM.render(
  	<Provider store={store}>
      	<App />
      </Provider>,
  	document.getElementById('root')
  )
  ```

- `createAction`

  ```javascript
  import { createAction } from '@reduxjs/toolkit'
  
  const increment = createAction('counter/increment')
  
  let action = increment()
  // { type: 'counter/increment' }
  
  action = increment(3)
  // returns { type: 'counter/increment', payload: 3 }
  
  console.log(increment.toString())
  // 'counter/increment'
  
  console.log(`The action type is: ${increment}`)
  // 'The action type is: counter/increment'
  ```

  - 기존의 Redux의 경우에는 action의 type과 액션의 타입을 구성하는 creator 함수를 따로 정의해야 했음
  - 이 두가지를 하나로 합칠 수 있게 만들어줌

- `createReducer`

  ```javascript
  import { createAction, createReducer } from '@reduxjs/toolkit'
  
  const increment = createAction('counter/increment')
  const decrement = createAction('counter/decrement')
  
  const counterReducer = createReducer(0, (builder) => {
    builder.addCase(increment, (state, action) => state + action.payload)
    builder.addCase(decrement, (state, action) => state - action.payload)
  })
  ```

  - `toString()`의 override로 인해 action 생성기는 reducer를 위한 key를 바로 return 받을 수 있음

- `createAsyncThunk`

  ```javascript
  import { createAsyncThunk, createSlice } from '@reduxjs/toolkit'
  import type { Pokemon } from './types'
  import type { RootState } from '../store'
  
  export const fetchPokemonByName = createAsyncThunk<Pokemon, string>(
    'pokemon/fetchByName',
    async (name, { rejectWithValue }) => {
      const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${name}`)
      const data = await response.json()
      if (response.status < 200 || response.status >= 300) {
        return rejectWithValue(data)
      }
      return data
    }
  )
  
  // slice & selectors omitted
  ```

  - 비동기적인 request lifecycle을 관리하기 위함
  - 액션 생성 함수
  - 첫번째 파라미터는 액션명 두번째는 콜백 함수
  - `createSlice`

  ```javascript
  // imports & thunk action creator omitted
  
  type RequestState = 'pending' | 'fulfilled' | 'rejected'
  
  export const pokemonSlice = createSlice({
    name: 'pokemon',
    initialState: {
      dataByName: {} as Record<string, Pokemon | undefined>,
      statusByName: {} as Record<string, RequestState | undefined>,
    },
    reducers: {},
    extraReducers: (builder) => {
      // When our request is pending:
      // - store the 'pending' state as the status for the corresponding pokemon name
      builder.addCase(fetchPokemonByName.pending, (state, action) => {
        state.statusByName[action.meta.arg] = 'pending'
      })
      // When our request is fulfilled:
      // - store the 'fulfilled' state as the status for the corresponding pokemon name
      // - and store the received payload as the data for the corresponding pokemon name
      builder.addCase(fetchPokemonByName.fulfilled, (state, action) => {
        state.statusByName[action.meta.arg] = 'fulfilled'
        state.dataByName[action.meta.arg] = action.payload
      })
      // When our request is rejected:
      // - store the 'rejected' state as the status for the corresponding pokemon name
      builder.addCase(fetchPokemonByName.rejected, (state, action) => {
        state.statusByName[action.meta.arg] = 'rejected'
      })
    },
  })
  
  // selectors omitted
  ```

  



## 출처

- https://redux.js.org/introduction/
