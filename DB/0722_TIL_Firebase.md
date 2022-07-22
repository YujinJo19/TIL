### 목차
- [Firebase](#firebase)
  * [1. Firebase](#1-firebase)
    + [1-1. Firebase란](#1-1-firebase란)
    + [1-2. 장점](#1-2-장점)
    + [1-3. 단점](#1-3-단점)
  * [2. Firebase 시작하기](#2-firebase-시작하기)
    + [2-1. react에 Firebase 추가하기](#2-1-react에-firebase-추가하기)
    + [2-2. Firebase에 등록하기](#2-2-firebase에-등록하기)
    + [2-3. Google 로그인 연동하기](#2-3-google-로그인-연동하기)
  * [출처](#출처)



#  Firebase 

## 1. Firebase

### 1-1. Firebase란

- 2014년도에 구글에서 인수한 모바일, 웹 애플리케이션 개발 플랫폼
- 백엔드 기능을 클라우드 서비스 형태로 제공하기 때문에 서버리스 애플리케이션 개발이 가능함



### 1-2. 장점

- NoSQl 기반의 3세대 데이터베이스
- RTSP(Real Time Stream Protocol)
  - 스트리밍 미디어 서버를 컨트롤하기 위한 통신시스템 등을 위해 고안된 네트워크 프로토콜

- 인증시스템 지원
- 원격 구성 지원
  - 앱의 환경을 원격으로 구성할 때 사용하는 기능
    - 앱의 배경화면 테마나 폰트 변경, 업데이트 창 알림창

- 콘솔 제공
  - 서버 관리자 페이지

- Analytics 제공
  - 현재 접속자, 오류 통계, 사용자 유지율, 앱 업데이트 상태 등을 추적할 수 있음




### 1-3. 단점

- 응답 속도가 느려짐



## 2. Firebase 시작하기

### 2-1. react에 Firebase 추가하기

```bash
$ yarn add firebase
```



### 2-2. Firebase에 등록하기

- 새로운 프로젝트를 생성하고  firebase에 연결하기

- react에 파일 추가

  ```javascript
  // Import the functions you need from the SDKs you need
  import { initializeApp } from "firebase/app";
  import { getAnalytics } from "firebase/analytics";
  // TODO: Add SDKs for Firebase products that you want to use
  // https://firebase.google.com/docs/web/setup#available-libraries
  
  // Your web app's Firebase configuration
  // For Firebase JS SDK v7.20.0 and later, measurementId is optional
  const firebaseConfig = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: "",
    measurementId: ""
  };
  
  // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  const analytics = getAnalytics(app);
  ```



### 2-3. Google 로그인 연동하기

```javascript
// Import the functions you need from the SDKs you need
import firebase from "firebase/app";
import "firebase/auth";

const firebaseConfig = {
  apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
  authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
  storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.REACT_APP_FIREBASE_APP_ID,
  measurementId: process.env.REACT_APP_FIREBASE_MEASUREMENT_ID
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);

export const auth = firebase.auth()

export const signInGoogle= () => {
  const provider = new firebase.auth.GoogleAuthProvider();
  return auth.signInWithPopup(provider);
}
```



## 출처

- https://12bme.tistory.com/345
- https://beomseok95.tistory.com/106
- https://velog.io/@realryankim/FireBase%EB%9E%80