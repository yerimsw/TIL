## 목차

0. [클래스 간 협력관계](#0-클래스-간-협력관계)
1. JwtTokenizer
2. CustomUserDetailsService
3. JwtAuthenticationFilter
4. SecurityConfiguration
<br>

### 0. 클래스 간 협력관계
![spring_security_jwt](https://user-images.githubusercontent.com/54367532/204087720-bfed8b04-de7a-4b06-b3fe-39eec7fa704c.png)
1. 클라이언트가 username과 password 정보를 전달한다.
2. `CustomAuthenticationFilter`는 `AuthenticationManager`에게 위 정보를 넘겨 인증처리를 위임한다.
3. `CustomUserDetailsService`를 통해 위 정보에 해당하는 DB 사용자를 조회한다.
4. DB에 저장되어있는 사용자를 UserDetails 인스턴스로 `AuthenticationManager`에게 반환한다.
5. `AuthenticationManager` 내부에서 클라이언트로부터 얻은 인증정보와 DB UserDetails 정보를 비교하여 인증처리를 진행한다.
6. 성공, 실패 여부를 `CustomAuthenticationFilter`에게 넘겨준다.
7. 성공, 실패 여부에 따라 클라이언트에게 JWT토큰을 발행한다.
<br>

![spring_security_jwt_filter](https://user-images.githubusercontent.com/54367532/204087693-9b90bd64-0912-48cb-ba0f-5acecf7c553f.png)
1. `CustomFilterConfigurer` 클래스 내에서 CustomAuthenticationFilter에 대한 아래와 같은 설정정보를 지정한다.
   - AuthenticationSuccessHandler: 인증 성공 시 실행 로직
   - AuthenticationFailureHandler: 인증 실패 시 실행 로직
   - FilterProcessUrl: 인증 처리를 진행할 URL

2. SecurityFilterChain에 `CustomAuthenticationFilter`를 적용시켜 특정 url에 대해 인증처리를 진행하도록 한다.
