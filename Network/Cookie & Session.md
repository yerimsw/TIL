## 목차
1. [Cookie](#1-cookie)
2. [Session](#2-session)
<br>

## 1. Cookie

> Stateless한 인터넷을 stateful 하게 사용할 수 있는 방법

- 서버측에서 클라이언트 쪽에 데이터를 저장하는 방법 중 하나이다.
- 서버는 클라이언트에서 쿠키를 이용해 데이터를 가져올 수 있다.
- 단 특정 조건들을 만족하는 경우에만 쿠키를 가져올 수 있는데, 이러한 조건들을 쿠키 옵션이라고 한다.
<br>

### 쿠키 옵션

1. Domain
   - URL의 서브 도메인, 세부 경로 등을 제외한 도메인을 의미한다.
   - `www.google.com` 에서 `google.com`이 domain이 된다.
   - 서버의 도메인과 쿠키의 도메인 정보가 일치해야 쿠키를 전송할 수 있다.
   - google.com 에서 받은 쿠키를 naver.com에 전송하는 일을 방지한다.

2. Path
   - 요청 URL이 `https://github.com/yerimsw/TIL/Network` 이라면 세부경로 Path는 `/yerimsw/TIL/Network`이다.
   - Path 옵션이 존재할 경우, 요청 URL의 세부경로와 쿠키의 세부경로가 일치해야 쿠키를 전송할 수 있다.
   - 쿠키 Path: `/yerimsw` , 요청 URL: `/yerimsw/TIL` 처럼 URL에 세부 경로가 더 추가된 경우에도 쿠키를 전송할 수 있다.

3. MaxAge & Expires
   - 쿠키가 유효한 기간을 정하는 옵션이다.
   - MaxAge는 앞으로 몇 초간 쿠키를 유지하는지 지정하고, Expires는 언제까지 유효할지 Date를 지정한다.
   - 세션 쿠키: MaxAge or Expires 옵션이 없는 쿠키로, 브라우저 종료 시 쿠키가 삭제된다.
   - 영속성 쿠키: 브라우저의 종료 여부와 관계 없이 MaxAge or Expires 옵션 설정 값 만큼 쿠키가 유효하다.

4. Secure
   - Secure 옵션을 true로 지정하면 HTTPS 프로토콜로 통신할 때만 쿠키를 전송할 수 있다.
   - Secure 옵션이 없다면 HTTP와 HTTPS 프로토콜 모두 쿠키를 전송할 수 있다.

5. HttpOnly
   - 스크립트에서 쿠키로의 접근 가능 여부를 지정한다.
   - true로 지정하면 스크립트에서 쿠키로의 접근이 불가능하다.
   - false 옵션일 경우 XSS 공격에 취약해진다.

6. SameSite
   - Lax: Cross Origin 요청이면 Get 메서드에 한해서 쿠키를 전송할 수 있다.
   - Strict: Cross Origin이 아닌 Same site(도메인, 프로토콜, 포트가 같음)인 경우에만 쿠키를 전송할 수 있다.
   - None: 항상 쿠키를 전송할 수 있다. 단, HTTPS 프로토콜로 통신해야한다.
<br>

## 2. Session

> 클라이언트가 인증에 성공한 상태

- 세션의 존재 여부에 따라 클라이언트가 리소스에 접근할 수 있는 권한이 달라진다.
- 서버는 사용자의 인증 성공 여부를 알아야 하고, 사용자는 서버의 인증 검증 수단을 갖고있어야 한다.
- 서버는 사용자가 인증에 성공하면 session을 저장하고, 각 세션에 session id를 부여한다.
- 이 session id를 클라이언트에게 쿠키와 함께 전송하므로, 쿠키에 session id 정보가 없을 경우 해당 요청이 인증되지 않았음을 판단한다.
