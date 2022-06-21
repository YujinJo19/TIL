#### 목차

- [Post CSS](#post-css)
  * [1. 특징](#1-특징)
    + [1-1.  Autoprefixer](#1-1--autoprefixer)
    + [1-2. PostCSS Preset Env](#1-2-postcss-preset-env)
    + [1-3. CSS Modules](#1-3-css-modules)
    + [1-4. stylelint](#1-4-stylelint)
  * [2. Plugin](#2-plugin)
  * [출처](#출처)



# Post CSS

- CSS의 전처리기 중 하나
- LESS, SASS 와 비슷



## 1. 특징

### 1-1.  Autoprefixer

- post css를 사용할 경우 prefix를 직접 작성하지 않아도 css로 변환하는 과정에서 자동으로 prefix를 넣어줌

  ```css
  // Post CSS
  :fullscreen {
  }
  
  // CSS
  :-webkit-full-screen {
  }
  :-ms-fullscreen {
  }
  :fullscreen {
  }
  ```

### 1-2. PostCSS Preset Env

- 최신 css를 브라우저가 이해할 수 있는 코드로 변환해줌

### 1-3. CSS Modules

- css를 모듈화해서 동일한 클래스명을 사용하더라도 각각 css가 적용됨
- 기존 css의 경우 클래스명이 같으면 마지막에 작성된 css가 적용되지만 post css로 모듈화할 경우 각 모듈에 맞는 css가 적용됨
- 클래스명을 문자열로 적지 않고 {} 안에 적음 
  - style.button

### 1-4. stylelint

- css의 잘못된 코드가 작성되었을 경우 콘솔에서 해당 오류를 확인할 수 있음



## 2. Plugin

- https://www.postcss.parts/
- https://github.com/postcss/postcss/blob/main/docs/plugins.md
- 다양한 플러그인이 존재해 필요한 기능들만 추가해서 사용할 수 있음



## 출처

- https://postcss.org/