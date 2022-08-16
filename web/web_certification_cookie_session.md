# HTTP 기본 인증 방식 2. Session/Cookie 방식
> = 모바일/웹의 인증을 책임지는 대표주자

계정 정보를 매번 HTTP 요청에 넣어 보내기엔 보안에 큰 단점이 있다. 이를 보완하기 위해 나온 방식이다.

<br>

## 인증 과정

1. 클라이언트에서 로그인을 한다.
2. 서버에서는 계정 정보를 읽어 사용자를 확인 → 사용자의 고유한 ID값을 부여하고 세션 저장소에 저장 → 이와 연결되는 **세션 ID** 발행
3. 클라이언트는 서버에서 해당 세션ID를 받아 **쿠키**에 저장 → 인증이 필요한 요청마다 쿠키를 헤더에 실어 보냄
4. 서버에서는 쿠키를 받아 세션 저장소에서 대조 → 대응되는 정보를 가져옴
5. 인증이 완료되고, 서버는 사용자에 맞는 데이터를 보냄

> **세션** : 서버에서 가지고 있는 정보
> **쿠키** : 사용자에게 발급된 세션을 열기 위한 열쇠(Session ID)

<br>

## 특징
- 기본적으로 **세션 저장소**를 필요로 함(주로 <span style='background-color:#fff5b1'>Redis</span> 이용)
  <span style='color:#808080'>*Redis : 고성능 key-value 저장소. NOSQL</span>
    - **세션 저장소**의 역할
        - 로그인시 ⇒ 사용자의 정보를 저장하고, 열쇠가 되는 세션 ID값을 만듦
        - 클라이언트가 요청에 쿠키를 넣어 보낼 시 ⇒ 저장되어있는 정보와 받은 쿠키를 매칭시켜 인증을 완료함

<br>
<br>


> ### 💡 세션 인증 vs 쿠키 인증
>
> **쿠키만으로 인증을 사용**한다 = 서버의 자원은 사용하지 않는다는 의미이다. 즉, 클라이언트가 인증 정보를 모두 책임지게 된다는 것이다. 이는 1.의 방식처럼 보안이 취약하다.
> > 👉🏻 따라서 쿠키만 사용하는 방식은 장바구니, 자동 로그인 설정 등(보안과 거리가 먼 기능)에서 유용하게 쓰인다!
>
> 결과적으로, **세션**을 **사용**하는 이유는 인증의 책임을 **서버**가 지게 하기 위함이다. (아무래도 클라이언트단보단 서버가 해킹당하기 훨씬 어렵기 때문!)
>
> 따라서 세션/쿠키 방식의 인증은
> - **클라이언트**는 **쿠키**를 이용,
> - **서버**에서는 **쿠키**를 받아 **세션**의 정보에 접근하는 방식
>
> 으로 진행된다.

<br>

## 쿠키/세션 방식의 장점

- 쿠키를 매개로 인증을 거침 → 보안이 비교적 안전함<br>

  쿠키가 담긴 HTTP request가 노출되더라도 쿠키 자체는 유의미한 값을 가지고 있지 않기 때문<br>
  <br>
- 사용자는 각기 고유의 ID값을 발급받음 → 서버의 자원 접근에 용이<br>
  서버가 쿠키값을 받았을 때 일일이 회원정보를 확인할 필요 없이 바로 어떤 회원인지 확인할 수 있기 때문

<br>

## 쿠키/세션 방식의 단점
- 세션 하이재킹(Session Hijacking) : 해커가 클라이언트의 HTTP 요청을 가로채면, 그 안의 쿠키도 훔칠 수 있음 → 그 훔친 쿠키를 이용하여 HTTP 요청을 보냄 → 서버의 세션 저장소에서는 정보를 주게 됨<br>

  > 👉🏻 **해결책**
  >   1. **HTTPS를 사용** → 요청 자체를 탈취해도 안의 정보를 읽기 어려움
  >   2. 세션에 **유효시간** 주기

    <br>
- 서버 부하 증가 : 서버에서 세션 저장소 사용 → 추가적인 저장공간을 필요로 하게 됨
