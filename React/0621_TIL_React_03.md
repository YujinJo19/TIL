#### 목차

- [React](#react)
  * [1. Class & Fuction](#1-class---fuction)
    + [1-1. Class](#1-1-class)
    + [1-2. Fuction](#1-2-fuction)
  * [2. React Hook](#2-react-hook)
    + [2-1. 사용법](#2-1-사용법)
  * [출처](#출처)



# React

## 1. Class & Fuction

- componentDidUpdate() 와 같이 컴포넌트가 업데이트 될 때마다 실행되는 함수 안에 무거운 로직을 작성해두었을 경우 불필요한 render 함수의 호출이 발생할 수 있음
  - Class의 PureComponent 와 함수의 memo는 Component와 state에 변화가 없다면 render 함수를 호출하지 않음
- `shouldComponentUpdate(nextProps, nextState)` 는 shallow comparison하기 때문에 동일한 reference를 가지고 있다면 내부의 데이터가 변경되어도 re-render되지 않음

### 1-1. Class

- React.Component

- React.PureComponent

  ```jsx
  import React, { PureComponent } from 'react';
  
  class Habit extends PureComponent {
  };
  
  export default Habit;
  ```

  - component와 state가 변화하지 않을 경우 render함수를 호출하지 않음

- 클래스가 생성될 때 멤버변수들은 한 번만 할당되고 render가 반복적으로 호출됨

### 1-2. Fuction

- function

- memo(function)

  ```jsx
  import React, { memo } from 'react';
  
  const HabitAddForm = memo(props => {
  });
  
  export default HabitAddForm
  ```

- React Hook

- 컴포넌트가 변경되면 코드 블럭 전체가 반복적으로 호출됨



## 2. React Hook

### 2-1. 사용법

```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

- `useState`
  - 기존 함수의 경우 state를 관리할 수 없었지만 react hook의 useState를 사용하면 state를 관리할 수 있음
  - 리스트의 첫번째 요소는 state로 관리할 정보, 두번째 요소는 해당 정보를 업데이트할 함수
  - 괄호 안에는 기본값을 넣어줌
  - 반복해서 함수가 호출되어도 useState의 기본값으로 돌아가지 않는 이유
    - react에서 해당 값을 저장하고있기 때문에 반복해서 호출한다고 해서 값이 기본값으로 바뀌진 않음



```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

- `useEffect` 

  - 기존 componentDidMount와 componentDidUpdate를 합친 것과 같음

  ```jsx
  // 1. 
  useEffect(() => {
      // Update the document title using the browser API
      document.title = `You clicked ${count} times`;
  });
  
  // 2. [count]
  useEffect(() => {
      // Update the document title using the browser API
      document.title = `You clicked ${count} times`;
  }, [count]);
  
  // 3. []
  useEffect(() => {
      // Update the document title using the browser API
      document.title = `You clicked ${count} times`;
  }, []);
  ```

  - 1번 : 경우 state가 업데이트될 때마다, 처음 mount될 때 호출
  - 2번 : count가 업데이트될 때마다, 처음 mount될 때 호출
  - 3번 : 처음 mount될 때만 호출



```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

- `useRef`
  - 기존의 createRef를 대신해서 사용하는 것
  - createRef 와 달리 계속해서 참조값을 만드는 것이 아니라 메모리에 저장해두고 재사용
  - useRef는 현재 참조 값의 변경 사항을 알려주지 않음



```jsx
function MeasureExample() {
  const [height, setHeight] = useState(0);

  const measuredRef = useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height);
    }
  }, []);

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  );
}
```

- `useCallback`
  - 함수를 momoization하기 위해 사용



## 출처

- https://reactjs.org/docs/getting-started.html