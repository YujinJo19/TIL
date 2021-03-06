### 목차
- [JWT](#jwt)
  * [1. 인증 방식](#1-인증-방식)
    + [1-1. Cookie](#1-1-cookie)
    + [1-2. Session](#1-2-session)
    + [1-3. Token](#1-3-token)
  * [2. JWT (Json Web Token)](#2-jwt--json-web-token-)
    + [2-1. JWT 구조](#2-1-jwt-구조)
    + [2-2. JWT 인증 순서](#2-2-jwt-인증-순서)
    + [2-3. JWT 장단점](#2-3-jwt-장단점)
  * [3. Refresh Token](#3-refresh-token)
    + [3-1. Refresh Token](#3-1-refresh-token)
    + [3-2. 재발급 원리](#3-2-재발급-원리)
  * [출처](#출처)



#  JWT 

## 1. 인증 방식

### 1-1. Cookie

- key - value 형식의 문자열 덩어리
- 서버를 통해 클라이언트의 브라우저에 설치되는 작은 기록 정보 파일을 말함
- Cookie 인증 순서
  1.  클라이언트가 서버에 요청을 보냄
  2. 서버는 클라이언트 측에 저장하고 싶은 정보를 응답 헤더의 Set-Cookie에 담아 보냄
  3. 클라이언트는 요청을 보낼 때마다 저장된 쿠키를 요청 헤더의 Cookie에 담아 보냄

- Cookie 방식의 단점
  - 쿠키 값을 그대로 보내기 때문에 보안에 취약
  - 용량 제한이 있음
  - 브라우저 간 공유가 불가능



### 1-2. Session

- 민감한 인증 정보를 브라우저가 아닌 서버 측에 저장하고 관리
- Key에 해당하는 SESSION ID와 이에 대응하고 Value로 구성되어 있음
- Value에는 세션 생성시간, 마지막 접근 시간 및 User가 저장한 속성 등이 Map 형태로 저장됨

- Session 인증 순서
  1. 유저가 웹사이트에서 로그인하면 세션이 서버 메모리 상에 저장됨
  2. 서버에서 브라우저 쿠키에 Session Id를 저장
  3. 브라우저는 모든 Request에 Session Id를 쿠키에 담아 전송
  4. 서버는 클라이언트가 보낸 Session Id와 서버 메모리로 관리하고 있는 Session Id와 비교하여 인증 수행
- Session 방식의 단점
  - Session Id 자체를 탈취하여 클라이언트인척 위장할 수 있음
  - 서버에서 세션 저장소를 사용하므로 요청이 많아지면 서버 부하가 심해짐



### 1-3. Token

- 클라이언트가 서버에 접속했을 때 서버가 해당 클라이언트에게 인증되었다는 의미로 토큰을 부여
- 토큰은 유일하며 요청 헤더에 토큰을 심어서 보냄
- 세션과 달리 서버가 아닌 클라이언트에 저장되기 때문에 서버의 부담이 감소
- Token 인증 순서
  1. 사용자가 로그인
  2. 서버 측에서 클라이언트에게 토큰 발급
  3. 클라이언트는 전달받은 토큰을 쿠키나 스토리지에 저장해두고 서버에 요청을 할 때마다 해당 토큰을 HTTP 요청 헤더에 포함시켜 전달
  4. 서버는 토큰을 검증하고 요청에 응답

- Token 방식의 단점
  - 토큰 자체의 데이터 길이가 길어서 요청이 많아질수록 네트워크 부하가 심해짐
  - payload는 암호화되지 않기 때문에 중요한 정보는 담을 수 없음
  - 토큰을 탈취당하면 대처하기 어려움



## 2. JWT (Json Web Token)

- 인증에 필요한 정보들을 암호화시킨 JSON 토큰
- JWT를 HTTP 헤더에 실어 서버가 클라이언트를 식별하는 방식
- JSON 데이터를 Base64 URL-safe Encode를 통해 인코딩하여 직렬화한 것이며, 위변조 방지를 위해 캐인키를 통한 전자서명도 들어있음



### 2-1. JWT 구조

- '.' 을 구분자로 나누어지는 세가지 문자열의 조합
- `Header.Payload.Signature`
- Header : JWT에서 사용할 타입과 해시 알고리즘의 종류
- Payload : 서버에서 첨부한 사용자 권한 정보와 데이터
- Signature : Header, Payload 를 Base64 URL-safe Encode 를 한 이후 Header 에 명시된 해시함수를 적용하고, 개인키(Private Key)로 서명한 전자서명이 담겨있음
- 전자서명에는 비대칭 암호화 알고리즘을 사용하므로 암호화를 위한 키와 복호화를 위한 키가 다름
  - 암호화에는 개인키를 복호화에는 공개키를 사용



### 2-2. JWT 인증 순서

1. 사용자가 서버에 로그인 인증을 요청
2. 서버가 인증 요청을 받으면 Header, PayLoad, Signature를 정의, 이를 각각 암호화하여 JWT를 생성하고 이를 쿠키에 담아 클라이언트에게 발급
3. 클라이언트는 서버로부터 받은 JWT를 로컬 스토리지에 저장(다른 콧에 저장 가능), API를 서버에 요청할 대 Authorization header에 Access Toke을 담아 보냄
4. 서버는 JWT가 서버에서 발행한 토큰인지 일치 여부를 확인하여 인증을 통과시켜 줌, 인증이 되면 payload에 들어있는 유저의 정보를 클라이언트에게 돌려줌
5. 엑세스 토큰의 시간이 만료되면 클라이언트는 리프레시 토큰을 이용해서
6. 서버로부터 새로운 엑세스 토큰을 발급 받음



### 2-3. JWT 장단점

- 장점

  -  데이터 위변조를 막을 수 있음
  - 별도의 저장소가 필요없음
  - 서버는 StateLess
  - 확장성이 우수

- 단점

  - 쿠키/세션과 다르게 토큰의 길이가 길어 요청이 많아질수록 부하가 심해짐

  - payload는 암호화되지 않기 때문에 중요한 정보는 담을 수 없음

  - 토큰을 탈취당하면 대처하기 어려움




## 3. Refresh Token

### 3-1. Refresh Token

- Access Token 만을 사용하게 되면 제 3자에게 탈취당할 경우 보안에 취약하다는 단점이 있음
- Access Token이 만료되기 전까지는 토큰을 획득한 사람이라면 권한 접근이 가능해지기 때문
- Refresh Token은 재발급에 관여하는 토큰
- 서버는 데이터베이스에 Refresh token을 저장하고 클라이언트는 Access Token과 Refresh Token을 쿠키, 세션 혹은 웹스토리지에 저장하고 요청이 있을 때마다 헤더에 담아 보냄
- 긴 유효기간을 지니면서 동시에 Access Token이 만료됐을 때 재발급해주는 열쇠가 됨
- 만료된 Access Token을 서버에 보내면 Refresh Token을 DB에 있는 것과 비교해 일치할 경우 다시 Access Token을 재발급해줌



### 3-2. 재발급 원리

- 로그인 시 Access Token과 Refresh Token을 모두 발급
- 토큰을 검사함과 동시에 토큰의 유효기간을 확인하여 재발급 여부를 결정
- 로그아웃을 하면 Access Token과 Refresh Token을 모두 만료시킴





## 출처

- https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC
- https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT