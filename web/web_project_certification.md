웹에서 인증 구현하기

---

### 웹개발에서 인증이란?
- 프론트엔드 관점 : 사용자의 로그인/회원가입 등 사용자의 도입부를 주로 가리킴
- 서버사이드 관점 : 모든 API 요청에 대해 사용자를 확인하는 작업

즉, 프론트에서 사용자가 누구인지 알 만한 단서를 서버에 보내주면, 서버는 그 단서를 파악하여 각 요청에 맞는 데이터를 반환하게 된다.

<br><br>


# HTTP

---
> 현재 모바일, 웹 서비스에서 가장 많이 쓰이는 통신 방식

- HTTP 통신은 응답 후 연결이 끊김 → 과거에 대한 정보를 전혀 담지 않음
- 지금 보내는 HTTP 요청은 지난 번에 내 정보를 담아 보냈던 HTTP 요청과 전혀 관계가 없음
- 각각의 HTTP 요청에는 주체가 누구인지에 대한 정보가 필수적임
(단, 인증이 필요 없다면 필수적이지 않을 수도)

<br>

## HTTP 메시지의 구조

---
> HTTP 메시지란?
>⇒ 서버에 요청(Request)을 보내는 것

![](https://velog.velcdn.com/images/sw_smj/post/63180a93-6ad2-46fa-b181-83a18f63fec2/image.png)

- 헤더와 바디 두 가지로 구성됨
- 공백 : 헤더와 바디를 구분짓는 역할
- **헤더** : 기본적으로 요청에 대한 정보들이 들어감
- **바디** : 서버로 보내야 할 데이터가 들어감

> 모바일/웹서비스의 인증 : 보통 HTTP 메시지의 **헤더**에 인증수단을 넣어 요청(Request) 보냄

<br><br>

## HTTP 기본 인증 방식

---

## 1. 계정정보를 요청 헤더에 넣는 방식
- 가장 보안이 낮은 방식 : HTTP의 Request에 인증할 수단으로 비밀번호를 넣음
- 보통 앱에서는 HTTP 요청시 따로 암호화되지 않음 → 해커의 intercept(HTTP 요청 가로채기)에 취약
- 절대 실제 서비스에서는 쓰이지 않음
- HTTPS : HTTP 요청을 암호화해서 보안을 높이는 방식. 이지만 그래도 안 쓰임

**장점**
- 인증을 테스트할 때 빠르게 시도해볼 수 있음

**단점**
- 보안에 매우 취약함
- 서버에서는 신호가 올 때마다 ID, PW를 통해 유저가 맞는지 인증해야 함 → 비효율적

<br><br>

## 2. Session/Cookie 방식
> = 모바일/웹의 인증을 책임지는 대표주자

1.처럼 계정 정보를 매번 요청에 넣어 보내기엔 보안에 큰 단점이 있다. 이를 보완하기 위해 나온 방식이다.

### 인증 과정

1. 클라이언트에서 로그인을 한다.
2. 서버에서는 계정 정보를 읽어 사용자를 확인 → 사용자의 고유한 ID값을 부여하고 세션 저장소에 저장 → 이와 연결되는 **세션 ID** 발행
3. 클라이언트는 서버에서 해당 세션ID를 받아 **쿠키**에 저장 → 인증이 필요한 요청마다 쿠키를 헤더에 실어 보냄
4. 서버에서는 쿠키를 받아 세션 저장소에서 대조 → 대응되는 정보를 가져옴
5. 인증이 완료되고, 서버는 사용자에 맞는 데이터를 보냄

> **세션** : 서버에서 가지고 있는 정보
> **쿠키** : 사용자에게 발급된 세션을 열기 위한 열쇠(Session ID)

<br>

### 특징
- 기본적으로 **세션 저장소**를 필요로 함(주로 <span style='background-color:#fff5b1'>Redis</span> 이용)
<span style='color:#808080'>*Redis : 고성능 key-value 저장소. NOSQL</span>
  - **세션 저장소**의 역할
  	- 로그인시 ⇒ 사용자의 정보를 저장하고, 열쇠가 되는 세션 ID값을 만듦
  	- 클라이언트가 요청에 쿠키를 넣어 보낼 시 ⇒ 저장되어있는 정보와 받은 쿠키를 매칭시켜 인증을 완료함

<br>

### 💡 세션 인증 vs 쿠키 인증

**쿠키만으로 인증을 사용**한다 = 서버의 자원은 사용하지 않는다는 의미이다. 즉, 클라이언트가 인증 정보를 모두 책임지게 된다는 것이다. 이는 1.의 방식처럼 보안이 취약하다.
> 👉🏻 따라서 쿠키만 사용하는 방식은 장바구니, 자동 로그인 설정 등(보안과 거리가 먼 기능)에서 유용하게 쓰인다!

결과적으로, **세션**을 **사용**하는 이유는 인증의 책임을 서버가 지게 하기 위함이다. (아무래도 클라이언트단보단 서버가 해킹당하기 훨씬 어렵기 때문!)

따라서 세션/쿠키 방식의 인증은
- 클라이언트는 쿠키를 이용,
- 서버에서는 쿠키를 받아 세션의 정보에 접근하는 방식

으로 진행된다.

### 장점

- 쿠키를 매개로 인증을 거침 → 보안이 비교적 안전함
쿠키가 담긴 HTTP request가 노출되더라도 쿠키 자체는 유의미한 값을 가지고 있지 않기 때문
- 사용자는 각기 고유의 ID값을 발급받음 → 서버의 자원 접근에 용이
서버가 쿠키값을 받았을 때 일일이 회원정보를 확인할 필요 없이 바로 어떤 회원인지 확인할 수 있기 때문

### 단점
- 세션 하이재킹(Session Hijacking) : 해커가 클라이언트의 HTTP 요청을 가로채면, 그 안의 쿠키도 훔칠 수 있음 → 그 훔친 쿠키를 이용하여 HTTP 요청을 보냄 → 서버의 세션 저장소에서는 정보를 주게 됨
**해결책**
	1. HTTPS를 사용 → 요청 자체를 탈취해도 안의 정보를 읽기 어려움
    2. 세션에 유효시간 주기
- 서버 부하 증가 : 서버에서 세션 저장소 사용 → 추가적인 저장공간을 필요로 하게 됨


<br><br>


## 3. 토큰(JWT) 기반 인증 방식

> 모바일/웹의 인증을 책임지는 또 다른 대표주자

- JSON 객체를 사용해 정보를 안정성 있게 전달하는 웹 표준
- 사용자는 Access Token(JWT 토큰)을 HTTP 헤더에 실어 서버로 보냄

### JWT
> JSON Web Token
> = 인증에 필요한 정보들을 암호화시킨 토큰

![](https://velog.velcdn.com/images/sw_smj/post/8aad2528-7924-4900-8203-6b27790332c2/image.png)

###### ▲ jwt.io 사이트에서 확인할 수 있는 암호화된 토큰

#### 토큰의 구성요소
📌 `토큰` = Encoded `Header` + `.` + Encoded `Payload` + `.` + `Verify Signature`
- `Header` : 암호화할 방식(alg), 타입(type) 등
- `Payload` : 서버에 보낼 데이터. (ex: 유저의 고유 ID값, 유효기간 등)
- `Verify Signature` : Base64방식으로 인코딩한 Header, payload 그리고 SECERT KEY를 더한 후 서명됨


> **❓ 토큰은 전체가 다 암호화되어 있나요?**
>    👉🏻 NO!
> - `Header`, `Payload` : 인코딩될 뿐(16진수로 변경), 따로 암호화되지 않음 ⇒ JWT 토큰에서 Header, Payload는 누구나 디코딩하여 확인 가능.
> - `Verify Signature` : Secret KEY를 알지 못하면 복호화할 수 없음 (즉, 다른 사용자가 Payload의 ID를 조작하여 보내도, Verify Signature가 일치하지 않기에 유효하지 않은 토큰으로 간주됨)

<br>

### JWT 기반 인증 과정
1. 클라이언트에서 로그인
2. 서버에서는 계정정보를 읽어 사용자 확인 → 사용자의 고유한 ID값 부여 → 기타 정보와 함께 Payload에 넣음
3. JWT 토큰의 유효기간 설정
4. 암호화할 SECRET KEY를 이용해 ACCESS TOKEN 발급
5. 클라이언트가 Access Token을 받아 저장 → 인증이 필요한 요청마다 토큰을 헤더에 실어 보냄
6. 서버에서는 받은 토큰의 Verify Signature를 SECRET KEY로 복호화 → 조작 여부, 유효기간 확인
7. 검증 완료 → Payload를 디코딩 → 사용자의 ID에 맞는 데이터를 가져옴

<br>

> 💡 **쿠키/세션 방식과 가장 큰 차이**
> - **세션/쿠키** : 세션 저장소에 유저의 정보를 넣음
> - **JWT** : 토큰 안에 유저의 정보를 넣음

물론 이는 클라이언트 입장에서는 HTTP 헤더에 세션 ID/토큰을 실어 보내는 것으로 유사하지만, 서버 입장에서는 인증을 위해 암호화를 할 것인가 / 별도의 저장소를 이용할 것인가로 차이가 있다.

### 장점
- **간편함** : 세션/쿠키와 달리 별도의 저장소가 필요하지 않음
- **<span style='background-color:#fff5b1'>Stateless</span>한 서버를 만들기 적합** : JWT는 발급 후 검증만 하면 됨
<span style='color:#808080'>*Stateless : 어떠한 별도의 저장소도 사용하지 않는, 즉 상태를 저장하지 않는 것 ⇒ 서버 확장/유지,보수에 유리함</span>
- **뛰어난 확장성** : 토큰 기반으로 하는 다른 인증 시스템에 접근 가능

### 단점
- **이미 발급된 JWT에 대해서는 돌이킬 수 없음**
세션/쿠키의 경우, 만일 쿠키가 악의적으로 이용될 시 해당 세션을 삭제해버리면 된다. 하지만 JWT는 한 번 발급되면 유효기간 만료까지는 계속 사용이 가능하다.
**해결책** : 기존 Access Token의 유효기간을 짧게 하고, Refresh Token이라는 새로운 토큰을 발급한다.
- **제한적인 Payload 정보**
Payload는 따로 암호화되지 않기 때문에 유저의 중요한 정보는 담을 수 없음
- **긴 JWT의 길이 → 서버 자원 낭비 발생**
세션/쿠키 방식에 비해 길이가 김 → 인증이 필요한 요청이 많아질수록 서버의 자원 낭비 발생




