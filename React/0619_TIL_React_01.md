#### 목차

- [React에 들어가기 전에](#react에-들어가기-전에)
  * [1. framework vs library](#1-framework-vs-library)
    + [1-1. 프레임워크](#1-1-프레임워크)
    + [1-2. 라이브러리](#1-2-라이브러리)
    + [1-3. 차이점](#1-3-차이점)
  * [2. React](#2-react)
    + [2-1. 구조](#2-1-구조)
  * [3. Class vs Function Component](#3-class-vs-function-component)
    + [3-1. Class](#3-1-class)
    + [3-2. Function](#3-2-function)
    + [3-3. React Hook](#3-3-react-hook)
  * [4. JSX](#4-jsx)
  * [출처](#출처)



# React에 들어가기 전에

## 1. framework vs library

### 1-1. 프레임워크

- 특정 프로그램을 개발하기 위한 여러 요소들과 메뉴얼인 툴을 제공하는 프로그램
- 바닥에서부터 프로그래밍을 진행하는 것이 아니라 기본 뼈대와 툴들을 제공해줌
- ex) Django, Spring, Angular



### 1-2. 라이브러리

- 도구의 모음
- famework가 틀과 도구의 집합이라면 library는 도구를 의미
- ex) UI를 위한 도구 -> React



### 1-3. 차이점

- 프레임워크를 사용해서 개발을 진행하면 해당 프레임워크의 규약들을 지켜야함
- 라이브러리를 사용할 경우 원하는 도구들을 골라서 사용할 수 있음



## 2. React

### 2-1. 구조

- 컴포넌트로 웹 UI를 만드는 라이브러리
- 컴포넌트는 state와 render로 구성되어 있음

```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

- state가 변경될 때마다 render 함수가 호출됨
- Virtual DOM(VDOM)이 있어서 모든 컴포넌트들이 변경되는 것이 아니라 기존과 변경된 사항이 있는 컴포넌트들만 변경됨
- 컴포넌트들은 독립적이며 재사용이 가능함



## 3. Class vs Function Component

### 3-1. Class

- state를 가질 수 있으며 state가 변경될 때마다 자동으로 render 됨

- lifecycle method

  - 컴포넌트가 mount, unmout, update될 때 코드를 동작하게 함

    - mount : DOM이 생성되고 웹 브라우저 상에 나타나는 것
    - unmount : DOM에서 제거되는 것

    

### 3-2. Function

- state나 lifycycle method가 존재하지 않음



### 3-3. React Hook

- 함수도 클래스와 같이 상태관리가 가능하도록 만들어줌
- 클래스를 사용하지 않고 함수에 react hook을 추가해서 사용하는 이유
  - mount, unmout, update 될 때 사용되는 함수가 개별적으로 존재하기 때문에 중복되는 코드가 많음
  - 클래스 내부에서는 this를 사용해야 하며 binding에 문제가 생길 수 있음



## 4. JSX

- react에서 사용하는 javascript(필수적인 것은 아니나 많은 사람들이 사용함)

- html 태그를 사용할 수 있는 것과 동시에 변수를 선언하여 사용할 수 있음

  ```jsx
  const name = 'Josh Perez'
  const element = <h1>Hello, {name}!</h1>
  ```

- {} 괄호 안에 javascript 표현식들을 사용할 수 있다.

  - 2 + 1

  - user.firstName

  - formatName(user)

  ```jsx
  function formatName(user) {
    return user.firstName + ' ' + user.lastName;
  }
  
  const user = {
    firstName: 'Harper',
    lastName: 'Perez'
  };
  
  const element = (
    <h1>
      Hello, {formatName(user)}!
    </h1>
  );
  ```

- if문과 for문도 사용이 가능

  ```jsx
  function getGreeting(user) {
    if (user) {
      return <h1>Hello, {formatName(user)}!</h1>;
    }
    return <h1>Hello, Stranger.</h1>;
  }
  ```

- html vs jsx

  ```jsx
  //html
  <h1 class="title" onclick="move">Hello</h1>
  
  //jsx
  <h1 className="title" onClick="move">Hello</h1>
  ```

- 태그가 비어있다면 XML 처럼 바로 태그를 닫을 수 있음

  ```jsx
  const element = <img src={user.avatarUrl} />;
  ```

- 형제노드는 존재할 수 없음

  ```jsx
  // 불가능
  <h1>Hello!</h1>
  <h2>Good to see you here.</h2>
  
  // 가능
  <div>
  	<h1>Hello!</h1>
  	<h2>Good to see you here.</h2>
  </div>
  
  <React.Fragment>
  	<h1>Hello!</h1>
  	<h2>Good to see you here.</h2>
  </React.Fragment>
  
  <>
  	<h1>Hello!</h1>
  	<h2>Good to see you here.</h2>
  </>
  ```



## 출처

- https://engkimbs.tistory.com/673
- https://reactjs.org/docs/getting-started.html
- https://kyun2da.dev/react/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4%EC%9D%98-%EC%9D%B4%ED%95%B4/