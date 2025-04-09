## Keycloak 환경설정
### 1.가입하기
* 가입하기 활성화 
  * Keycloak 제공하는 기본 로그인 하면에서 하단에 '가입하기(Register)' link가 보이게 함
* Client 선택 후,
* Realm settings > Login 탭 선택
> User registration : ON<BR>
> Remember me : ON

### 2. 비밀번호 찾기
* Client 선택 후,
* Realm settings > Login 탭 선택
> Forgot password : ON<BR>

### 3. 회원가입시 이메일 인증
* Client 선택 후,
* Realm settings > Login 탭 선택
> Verify email : ON
> Duplicate emails : ON

### 3-1. SMTP 설정
* Client 선택 > Realm settins > Email 탬 선택
> From : admin@test.com (발송자 이메일)
> From display name : Admin 
> Host : smtp.gmail.com
> Port : 465
> Encryption : Enable SSL ON 
> Authentication : gmail ID/PW(앱 비밀번호 입력)
* 입력 후 'Test connection'을 눌러 메일 발송이 성공하면 저장한다.
* SMTP 가 설정이 되어야 회원가입시 이메일 인증이 가능
---

### 4. OTP 적용하기
### 개요
> 2 Factor 인증을 위하여 Login 과정에서 OTP 과정을 수행하게 Flow를 변경해 준다.
> Application Flow를 통해서 구현한다.

### OTP Flow 정의

#### Realm > Authentication
* Authentication > browser 선택
> browser flow : 브라우저 기반 인증시 Flow 정의

1. Built-in Flow는 수정할 수 없어 우측 상단의 Action > Duplicate를 선택
2. Built-in Flow를 My-Browser로 복사
3. Autentication > My-Browser 로 들어가서 수정
> 불필요한 flow 삭제
> Identity Provider Redirector : OFF <BR>
> Username / Password ~ OTP Form 까지 : 모두 ON

### OTP 적용하기
* Realm > Clients 선택
* Advanced 최하단 부분에서 my-browser 선택
> Authentication flow overrides
>  -> Browser Flow : My-Browser 선택

### 로그인시 OTP 등록
* 지원OTP : FreeOTP / Google Authenticator / MS Autenticator 3개
> ID / PWD 인증 후 최초 OTP 기기등록 하는 단계가 나타난다.
> Authenticator App에서 QR코드를 스캔하여 등록한다.
> 이후 ID/PWD 1차 인증 후 -> OTP 2차 인증을 수행해야 Login 가능하다.

### 주의 사항
* Site에 2차 인증이 적용되게 되면,
* 사용자를 1차 ID/PW 로그인 후 OTP 입력화면으로 이동된다.
* 이떄 처음 사용하는 경우는 Authenticator 를 등록하도록 QR 코드 화면이 보여진다.
* 최초 Authenticator 등록 후에는 2차 인증 단계인 OTP만 입력하면 로그인이 된다.
> 한번 OTP로 로그인 한 경우
> 로그인이 적용되지 않은 Site 를 방문해서 로그인을 수행할 때
> 이미 계정이 OTP 2차 인증을 수행하는 계정으로 속성이 변경되었기 떄문에
> 계속해서 2차 인증까지 수행을 하도록 강제화 되어 있음