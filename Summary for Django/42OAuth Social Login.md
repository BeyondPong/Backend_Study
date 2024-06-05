# JWT: Json Web Token

### Authentication: 인증
- 입력한 ID와 Password를 통해 사용자의 신원을 검증하는 절차
- 인증을 위해, 사전에 사용자의 정보들을 생성하고 저장할 수 있는 기능 필요
  (입력한 ID와 Password를 DB에 저장된 내용과 비교)

### Authorization: 인가
- 로그인 후 사용자가 요청한 `request`를 실행할 권한을 부여하는 과정
  1. 로그인 성공 시, `access token`을 클라이언트에게 전송
  2. 다음 `request` 부터는 `access token`을 첨부해서 서버에 요청 전송
  3. 서버는 `access token`을 통해 사용자의 권한을 확인
     해당 권한을 가지고 있으면 요청을 처리
     권한이 없으면, Unauthorized Response(401) 혹은 다른 에러 코드 반환

### JWT란?
: Java Script의 Object 자료구조를 가지고 있으며, `Web Token`으로써 사용할 수 있다.

> [!important]
> Q) **왜 JWT를 사용할까?**
> A) HTTP는 기본적으로 `state-less`를 지향한다
> 	서버-클라이언트 구조에서 서버가 클라이언트의 상태를 가지고 있지 않다는 의미
>
> *장점 1.* 서버의 확장성이 높으며 대량의 트래픽이 발생해도 대처할 수 있음
> *장점 2.* 서버가 분리되어 있는 경우, 특정 DB/서버에 의존하지 않아도 인증할 수 있음
>
> *단점 1.* `state-ful`(session) 방식보다 비교적 많은 양의 데이터가 반복적으로 전송되므로 네트워크 성능 저하가 될 수 있음
> *단점 2.* 데이터 노출로 인한 보안적인 문제 존재
>
> 따라서, 토큰에는 혹시 모를 상황으로 외부에 노출되어도 문제없는 가장 쓸모없는 데이터이며, 서버에서 유저를 식별할 수 있는 내용을 넣는다
>
> 사용자 인증을 저장해놓는 방식에는 가장 크게 jwt 토큰 발급 방식과 세션 발급 방식이 있는데...
> - 토큰 발급 방식: 몇 만명의 인증 방식을 저장하고 있지 않기 때문에, 가볍고 확장성이 좋음
> - 세션 발급 방식: 무겁지만, 그만큼 보안성이 좋음
> => 프로젝트의 기획과 시선에 따라 상호 대체 및 함께 사용이 가능하다

### JWT의 구성
```http
aaaaaaa.bbbbbbb.ccccccc //헤더.페이로드.시그니처
```

- 헤더(header)
	- 토큰 타입, 암호화 알고리즘 명시
- 페이로드(payload)
	- `jwt`에 넣을 데이터, 발급 및 만료일 명시
- 시그니처(secret)
	- 헤더와 페이로드가 변조되었는지 확인하는 역할

---
# Social Login Implementation

### Flow

1. `FE` -> Button(login) -> `42 login` -> login success -> authorize
2. `42 api` -> redirect_uri(localhost:5741?code=블라블라~) -> `FE`
3. `FE` -> POST(login/callback) with code -> `BE`
4. `BE` -> POST(OAUTH_TOKEN_URL) with code -> `42 api`
5. `42 api` -> accessToken, refreshToken, etc -> `BE`
6. `BE` -> GET(OAUTH_API_URL) with accessToken -> `42 api`
7. `42 api` -> user info -> `BE`
8. `BE` -> check is in DB(nickname) -> O(pass) or X(save in DB)
9. `BE` -> JWT(nickname, email, access_token) -> `FE` -> cookie~

### QnA

**Q1**) Login accessToken 보내야 할거 같은데, 안보냄??
**A1**) 응 당연히 안보내지. 코드를 통해 access token을 발급받고 로그인을 하고 회원가입의 경우 create을 하는건데?

**Q2**) 유저 확인 절차 어디서 해야해??
**A2**) code를 받아서 user info를 받아오면 그 내용으로 DB에 접근하는거란다.
	code -> accessToken -> userInfo
	userInfo -> user정보를 알게됨 -> verify
	=> 42api 받은 정보를 기반으로 우리 서버의 DB저장 해야 한다.

1. accessToken 만료일이 훠어어ㅓㄹ씬 빠름
	1. 요청을 할 때 마다 42api에 요청을해서 이게 맞는 유저인지 판단을 한다
	2. accessToken이 만료됐다는 에러가 뜨면
	3. refreshToken으로 재발급 요청을 하고
	4. accessToken을 다시 쿠키로 담아서 FE에 보낸다.
2. accessToken 만료될 때마다 유저에게 로그인하라고 한다(42가..그럼...). 42oauth 7200

**Q3**) JWT 개념은 알겠지만 감은 안옴..
**A3**) 토큰 등의 정보를 그냥 그대로 넘겨주면 프론트의 개발자도구에서 내용이 다보임(보안 상 문제)
	JWT : header payload, secret
	header, payload + server secret key => received JWT == maked JWT

**Q4**) middleware-> JWT middleware (걍 하면 되는거아님?, 왜안됨)
**A4**) Django에서는 middleware 놀랍게도 전역만 됨.
	하지만 우리는 로그인을 하지 않아도 로컬 게임 등이 가능해야 함
	-> 이런 불상사가 생기는데 나 로그인 할래, `Django` : jwt 없으니까 너 안됨
	-> 나 로그인 안했는데, jwt가 어떻게 있음? 하..
	-> 그럼 미들웨어 부분적 사용방법은?
	-> 커스텀 미들웨어가 있었는데, 잘 안먹어서
	=> 함수로 매번 선언하거나, @allowany와 비슷한 형태의 내용을 사용하면 될듯?

**Q5**) `2fa` ON/OFF는 도데체 어느 때에 해야함?
**A5**) 일단 우리는 ON/OFF가 아닌 항상 `2fa`를 하기로 함(가능하면 구글, 기본으로는 42-email)
	`FE`에서 로그인에 성공하는 순간 바로 2차 인증을 하는 화면으로 리다이렉트하여 `BE`에 요청
	`BE`에서 받은 정보로 이메일을 보내고 OK 응답
	`FE`에서 이메일로 받은 코드를 입력해서 `BE`에 확인 요청
	`BE`에서 코드가 보낸 내용이 맞는지 확인 후 OK 응답

jwt -> 2fa 인증여부 X 2fa O => `POST` (JWT 바꿀 것이기 때문: is_active 사용할것)
FE -> 쿠키 또 왔네 그냥 대입 해버리면됨
login -> 2fa ON
재로그인 -> 2fa 절차 실행

twofa/create, twofa/verify
2fa를 해야하는사람, 2fa 절차를 완료여부