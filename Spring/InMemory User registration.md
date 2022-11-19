## 목차

> Spring Security에서 InMemory User를 등록하는 방법을 알아보자.

1. [UserDetailsManager 빈 등록](#1-userdetailsmanager)
2. [PasswordEncoder 빈 등록](#2-passwordencoder)
3. [회원 가입 비즈니스 로직 작성](#3-inmemorymemberservice)
<br>

### 1. UserDetailsManager

```java
@Configuration
public class SecurityConfiguration {
  @Bean
  public InMemoryUserDetailsManager userDetailsManager() {
    UserDetails user =
          User.withDefaultPasswordEncoder()
              .username("user@email.com)
              .password("1111")
              .roles("USER")
              .build();
    return new InMemoryUserDetailsManager(user);
}
```

- Spring Security에서 User의 정보는 UserDetailsManager에서 관리하므로 빈으로 등록한다.
- UserDetails 인터페이스는 User의 정보를 포함한다.
<br>

### 2. PasswordEncoder

- PasswordEncoder는 Spring Security에서 패스워드 암호화 기능을 제공한다.
- 디폴트 암호화 알고리즘은 bcrypt이다.
- 사용자 password를 암호화 하기 위해 빈으로 등록한다.

```java
@Configuration
public class SecurityConfiguration {
  ...
  
  @Bean
  public PasswordEncoder passwordEncoder() {
    return new PasswordEncoderFactories.createDelegatingPasswordEncoder();
  }
}
```

- PasswordEncoderFactories가 DelegatingPasswordEncoder를 생성한다.
- DelegatingPasswordEncoder는 PasswordEncoder 구현 객체를 생성한다.
<br>

### 3. InMemoryMemberService

- 회원 가입 요청에 대한 비즈니스 로직을 작성한다.
- UserDetailsManager는 Spring Security에서 User를 관리하기 때문에 필요하고, PasswordEncoder는 User password를 암호화하기 위해 필요하다.

``` java
public class InMemoryMemberService implements MemberService {
  private final UserDetailsManager userDetailsManager;
  private final PasswordEncoder passwordEncoder;
  
  public InMemoryMemberService(UserDetailsManager userDetailsManager, PasswordEncoder passwordEncoder) {
    this.userDetailsManager = userDetailsManager;
    this.passwordEncoder = passwordEncoder;
  } // (1)
  
  @Override
  public Member createMember(Member member) {
    List<GrantedAuthority> authorities = createAuthorities(Member.MemberRole.ROLE_USER.name()); // (2)

    String encryptedPassword = passwordEncoder.encode(member.getPassword()); // (3)

    UserDetails userDetails = new User(member.getEmail(),encryptedPassword,authorities);
    userDetailsManager.createUser(userDetails); // (4)

    return member;
  }

  private List<GrantedAuthority> createAuthorities(String... roles) {
    return Arrays.stream(roles)
            .map(role -> new SimpleGrantedAuthority(role))
            .collect(Collectors.toList());
  }
}


}
```

1. UserDetailsManager와 PasswordEncoder를 DI 받는다.
2. Spring Security에서 User를 등록하기 위해 권한을 지정한다. `createAuthorities()` 메서드를 정의해 권한 목록을 `List<GrantedAuthority>` 타입으로 생성한다.
3. passwordEncoder의 `encode()` 메서드를 통해 패스워드를 암호화한다.
4. UserDetails를 생성하고 UserDetailsManager를 통해 사용자를 등록한다.

