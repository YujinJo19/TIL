#### 목차

- [React](#react)
  * [1. React-router](#1-react-router)
    + [1-1. 설치하기](#1-1-설치하기)
  * [2. Router 적용하기](#2-router-적용하기)
    + [2-1. 오류 코드](#2-1-오류-코드)
  * [출처](#출처)
  
  

# React

## 1. React-router

### 1-1. 설치하기

```bash
$yarn add react-router-dom@6
```

```javascript
// src/index.js

import {BrowserRouter} from "react-router-dom";


root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

```javascript
// src/App.js

import {Routes, Route, Link} from "react-router-dom";

functions App() {
    return (
    	<div className="App">
        	<h1>Welcome to React Router!</h1>
        	<Routes>
        		<Route path="/" element={<Home />} />
				<Route path="about" element={<About />} />
          	<Routes>
       	</div>
    );
}
```

- <Routes>
  - 여러 Route를 감쌀 수 있음
  - 내부의 Route 중 규칙이 일치하는 단 하나만 렌더링 시켜주는 역할
- <Route>
  - pathe 속성에 경로, element 속성에는 컴포넌트를 넣어 줌
  - 여러 라우팅을 매칭하고 싶은 경우 URL 뒤에 *을 사용하면 됨
- <Link>
  - a태그와 비슷하지만 페이지를 새로 불러오는 것이 아니라 브라우저 주소의 경로만 바꿔줌
  - `<Link to="경로">링크명</Link>`



## 2. Router 적용하기

### 2-1. 오류 코드

- Matched leaf route at location "/" does not have an element. This means it will render an <Outlet /> with a null value by default resulting in an "empty" page.
  - Route의 태그에서 'component' 사용 불가 => 'element'로 변경



## 출처

- https://reactrouter.com/docs/en/v6/getting-started/overview

- https://goddaehee.tistory.com/305
- https://github.com/remix-run/react-router/blob/main/docs/upgrading/v5.md#advantages-of-route-element