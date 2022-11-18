
### 1. Spring Security

- Spring MVC 기반 애플리케이션의 인증, 권한 부여 기능 등을 지원하는 보안 프레임워크이다.
- Spring Security가 기본적으로 지원하는 옵션을 통해 대부분의 보안 요구사항을 만족시킬 수 있다.
- 특정 보안 요구사항을 만족시킬 수 없을 때에도 코드의 유연한 확장이 가능하다.
<br>

```
implementation 'org.springframework.boot:spring-boot-starter-security'
```
- Spring security 사용을 위해 build.gradle 파일에 위 코드를 작성하여 의존 라이브러리를 추가해야한다.
<br>

```
Using generated security password: 39f49b79-fca1-4069-abff-e2126bd2ddd6
This generated password is for development use only.
Your security configuration must be updated before running your application in production.
```
- 디폴트 Username은 user, Password는 콘솔에 출력되는 로그를 통해 확인할 수 있다.
- 하지만 애플리케이션을 실행할 때 마다 패스워드가 변경되고, 개개인의 정보로 인증을 진행할 수 없다는 문제점이 있다.
- 위 콘솔 메시지에서도 출력되었 듯 Spring Configuration을 통해 인증 방식을 수정하면 위 문제를 해결할 수 있다.
<br>

### 2.UserDetails

``` java
@Configuration
public class SecurityConfiguration {
  @Bean
  public InMemoryUserDetailsManager userDetailsService() { // (1)
    
    UserDetails user = // (2)
            User.withDefaultPasswordEncoder() // (3)
                .username("email@email.com")
                .password("1234")
                .roles("USER")
                .build();
    
    return new InMemoryUserDetailsManager(user);
  }
}
```
1. `InMemoryUserDetailsManager` 클래스는 `UserDetailsManager` 인터페이스의 구현 클래스로, UserDetails를 관리한다.

2. `UserDetails` 인터페이스는 사용자 인증 정보들을 포함하고, `User` 클래스는 UserDetails의 구현 클래스이다.

3. `withDefaultPasswordEncoder()` 메서드는 password() 매개변수를 암호화 한다.
   - deprecated 되었는데, 이는 향후 버전에서 제거될 것을 의미하지 않고 production 환경에서 사용하지 말 것을 권고하기 위함이다.
<br>

### 3. SecurityFilterChain

- `HttpSecurity` 타입을 파라미터로 갖고 `SecurityFilterChain` 타입을 리턴하는 메서드를 정의하면 HTTP 보안 설정을 구성할 수 있다.
- HttpSecurity를 통해 HTTP 요청에 대한 보안 설정을 구성할 수 있다.

``` java
@Configuration
public class SecurityConfiguration {
  ...
  
  @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .formLogin() // (1)
        .loginPage("URL")
        .loginProcessingUrl("URL")
        .failureUrl("URL")
        .and() // (2)
        .exceptionHandling().accessDeniedPage("URL") // (3)
        .and()
        .authorizeHttpRequests(authorize -> authorize // (4)
            .antMatchers("URL1").hasRole("ADMIN")
            .antMatchers("URL2").permitAll()
        );
    
    return http.build();
  }
}
```
1. formLogin()
   - `formLogin()`을 통해 기본 인증 방법을 폼 로그인 방식으로 지정한다.
   - loginPage("URL") 으로 사용자 지정 로그인 페이지를 사용할 수 있다. URL에 매핑되는 핸들러 메서드는 로그인 페이지를 리턴한다.
   - loginProcessingUrl("URL")은 URL로 로그인 인증 요청을 보내고, failureUrl("URL")은 인증 실패 시 지정 URL로 redirect 한다.



