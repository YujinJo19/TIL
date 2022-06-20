#### 목차

- [React](#react)
  * [1. Handling Events](#1-handling-events)
    + [1-1. HTML vs React](#1-1-html-vs-react)
    + [1-2. Synthetic Events](#1-2-synthetic-events)
  * [2. State & Props](#2-state--props)
    + [2-1. State](#2-1-state)
    + [2-2. Props](#2-2-props)
  * [3. Refs and DOM](#3-refs-and-dom)
    + [3-1. Refs 사용법](#3-1-refs-사용법)
  * [4. 자주 쓰는 문법](#4-자주-쓰는-문법)
  * [출처](#출처)



# React

## 1. Handling Events

### 1-1. HTML vs React

- react에서 이벤트를 등록할 때 {} 안에 이벤트 함수 명을 넣어주어야 함

  ```jsx
  // HTML
  <button onclick="activateLasers()">
    Activate Lasers
  </button>
  
  // React
  <button onClick={activateLasers}>
    Activate Lasers
  </button>
  ```

- prevent default를 사용할 때 태그에서 바로 사용할 수 없고 함수안에서 사용할 수 있음

  ```jsx
  // HTML
  <form onsubmit="console.log('You clicked submit.'); return false">
    <button type="submit">Submit</button>
  </form>
  
  // React
  function Form() {
    function handleSubmit(event) {
      event.preventDefault();
      console.log('You clicked submit.');
    }
  
    return (
      <form onSubmit={handleSubmit}>
        <button type="submit">Submit</button>
      </form>
    );
  }
  ```

- 여기서 event는 react에서 사용하는 synthetic event

  - react의 synthetic evnet는 W3C를 따르고 있기 때문에 브라우저 간의 호환성을 걱정할 필요 없음

### 1-2. Synthetic Events

- 기존 DOM Event에서 한 단계 더감싸진 Event

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

- this.handleClick을 binding해주지 않을 경우 undefined	

- onClick 안에서 callback 형식으로 작성

  ```jsx
  class LoggingButton extends React.Component {
    handleClick() {
      console.log('this is:', this);
    }
  
    render() {
      // This syntax ensures `this` is bound within handleClick
      return (
        <button onClick={() => this.handleClick()}>
          Click me
        </button>
      );
    }
  }
  ```

  - 그렇지만 이렇게 작성했을 경우 prop을 받는 components에서 추가 렌더링이 발생할 수 있음

  

## 2. State & Props

### 2-1. State

- 컴포넌트 안에서 관리됨
- `this.state.stateName`으로 접근
- state를 업데이트할 때는 반드시 `setState` 함수를 호출해야 함
  - 그냥 업데이트를 할 경우 React는 state의 업데이트 여부를 알 수 없기 때문
  - 따라서 state를 직접 수정하는 것은 좋지 않음

### 2-2. Props

- 컴포넌트의 외부에서 데이터를 제공받음(부모 컴포넌트에서)

  - 컴포넌트의 재사용을 높이기 위해서 

- state에 리스트를 정의하고 prop해서 사용하는 경우 key를 지정해줘야함

  - `Each child in a list should have a unique "key" prop.`

  - ID 값이 있어야 React가 해당 값의 변화여부를 판단하는 데 도움이 되기 때문

  - 고유한 값이어야 하며 배열의 인덱스는 사용하지 않는 것이 좋음

    - 인덱스는 변경될 수 있기 때문

    

## 3. Refs and DOM

### 3-1. Refs 사용법

- 다른 리액트 요소에 접근하고 싶을 경우에 사용
  - DOM 요소에 바로 접근하지 않음

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

- input의 value에 접근하려면 `this.inputRef.current.value`



## 4. 자주 쓰는 문법

- ```
  array.forEach(callback(element[, index[, array]]))
  ```

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수는 3가지 매개변수로 구성
    - element: 배열의 요소
    - index: 배열 요소의 인덱스
    - array: 배열 자체
  - 반환 값(return)이 없는 메서드

- ```
  array.map(callback(element[, index[, array]]))
  ```

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 샐행
  - 콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환
  - 기존 배열 전체를 다른 형태로 바꿀 때 유용

- ```
  array.filter(callback(element[, index[, array]]))
  ```

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환
  - 기존 배열의 요소들을 필터링할 때 유용

- ```
  array.find(callback(element[, index[, array]]))
  ```

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 반환 값이 참이면, 조건을 만족하는 첫번째 요소를 반환
  - 찾는 값이 배열에 없으면 undefined 반환

- Spread operator

  - spread operator를 사용하면 객체 내부에서 객체 전개 가능
  - ES5까지는 Object.assign() 메서드를 사용
  - 얕은 복사에 활용 가능

  ```jsx
  const array = {apple: 1, banana: 2}
  
  const newArray = {...array, carrot: 3}
  
  console.log(newArray)	// {apple: 1, banana: 2, carrot: 3}
  ```

  

## 출처

- https://reactjs.org/docs/getting-started.html
- https://velog.io/@hidaehyunlee/React-State-%EB%9E%80