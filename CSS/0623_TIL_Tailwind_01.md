# Tailwind

- 부트스트랩과 비슷하게 grid나 m-1 같은 클래스를 이용하여 간편하게 스타일링 할 수 있도록 도와주는 프레임워크
- 사용자가 편리하게 custom해서 사용할 수 있음
- [공식문서](https://tailwindcss.com/docs/installation)



### Tailwind with React

```bash
# react project 생성
yarn create react-app app_name

# 프로젝트 폴더로 이동
cd app_name

# tailwindcss, postcss, autoprefixer 설치
yarn add tailwindcss postcss autoprefixer

# tailwind.config.js 파일 생성
npx tailwindcss init
```



```js
// tailwind.config.js
module.exports = {
 content: [
 "./src/**/*.{js,jsx,ts,tsx}",
 ],
 theme: {
 extend: {},
 },
 plugins: [],
}
```



```css
/* index.css */
/* @tailwind에 밑줄이 생길 경우 
	확장프로그램 PostCSS Language Support 설치(visual studio code) 
*/
@tailwind base;
@tailwind components;
@tailwind utilities;
```



## 출처

- https://tailwindcss.com/docs/installation
- https://www.makeuseof.com/tailwind-css-in-react/