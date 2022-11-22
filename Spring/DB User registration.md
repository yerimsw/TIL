## 목차

> DB 메모리에 User 인증정보를 등록하는 방법을 알아보자.

1. [DBMemberService](#1-dbmemberservice)
2. [CustomAuthorityUtils](#2-customauthorityutils)
3. [CustomUserDetailsService](#3-customuserdetailsservice)
<br>

### 1. DBMemberService

> 데이터베이스에 사용자를 등록하기 위해 DBMemberService 클래스를 생성한다.

- DBMemberService 클래스의 책임은 암호화된 User Credential과 권한 데이터를 포함하는 Member 객체를 DB에 저장하는 것이다.

```java
@Transactional
public class DBMemberService implements MemberService {

    private final MemberRepository memberRepository;
    private final PasswordEncoder passwordEncoder;
    private final CustomAuthorityUtils authorityUtils;

    public DBMemberService(MemberRepository memberRepository, PasswordEncoder passwordEncoder, CustomAuthorityUtils authorityUtils) {
        this.memberRepository = memberRepository;
        this.passwordEncoder = passwordEncoder;
        this.authorityUtils = authorityUtils;
    } // (1)

    @Override
    public Member createMember(Member member) {
        verifyExistsEmail(member.getEmail()); // 이미 존재하는 이메일인지 검증하는 메서드
        String encryptedPassword = passwordEncoder.encode(member.getPassword()); // 패스워드 암호화
        member.setPassword(encryptedPassword); // (2) 암호화 된 패스워드로 DB에 저장

        List<String> roles = authorityUtils.createRoles(member.getEmail()); // 이메일을 기준으로 사용자의 Role 구분
        member.setRoles(roles); // (3) Role에 따른 권한을 DB에 저장

        Member savedMember = memberRepository.save(member);

        System.out.println("#Memeber Created in DB");
        return savedMember;
    }
}

// MemberService 인터페이스에는 createMember(Member member) 추상메서드가 존재
// createMember에서 해야할 일은 패스워드를 암호화 해서 DB에 저장할 것.
```

1. DB 연동을 위해 `MemberRepository`를, 패스워드 암호화를 위해 `PasswordEncoder`를, User에게 권한을 부여하기 위해 `AuthorityUtils`를 DI 받는다.
2. `PasswordEncoder`로 암호화한 패스워드를 DB에 저장한다.
3. AuthorityUtils로 User의 Role에 따른 User 권한 설정을 하여 DB에 저장한다.
<br>

### 2. CustomAuthorityUtils

> DB상에 저장 될 User의 role과 role에 따른 권한을 매핑한다.

- 위 DBMemberService 클래스의 createMember 메서드 내 `List<String> roles`를 반환하기 위해 사용된다
  
```java
@Component // User객체에 넘겨줄 권한 컬렉션을 Role 기반으로 매핑, 생성한다.
public class CustomAuthorityUtils {

    @Value("${mail.address.admin}") // (1) application.yml에 추가한 프로퍼티를 가져오는 표현식
    private String adminMailAddress;
    
    private final List<String> ADMIN_ROLES_STRING = List.of("ADMIN","USER");
    private final List<String> USER_ROLES_STRING = List.of("USER");

    public List<String> createRoles(String email) {
        if(email.equals(adminMailAddress)) {
            return ADMIN_ROLES_STRING;
        }
        return USER_ROLES_STRING;
    } // (2)
    
    public List<GrantedAuthority> createAuthorities(List<String> roles) {
        List<GrantedAuthority> authorities = roles.stream()
                .map(role -> new SimpleGrantedAuthority("ROLE_" + role))
                .collect(Collectors.toList());
        return authorities;
    } // (3)
}
```
  
1. application.yml의 프로퍼티를 코드에서 사용할 수 있다.
2. User의 mail 주소에 따라 Role 역할을 부여한다.
3. DB에 저장된 Role을 기반으로 권한 정보를 생성한다.
<br>

- 지금까지의 코드로 DB 메모리에 User Cridential과 권한이 포함된 Member 객체를 저장할 수 있다.
- 그러나 아직 User Authentication이 Spring Security에 의해 진행되지 않은 상태이다.
- 이는 `UserDetailsService`에 User 인증정보 `UserDetails`를 넘겨주어 인증 절차를 진행할 수 있다.
<br>

### 3. CustomUserDetailsService

- UserDetailsService는 User의 정보를 가져오는 인터페이스로, 구현 클래스에서는 `loadUserByUsername` 메서드를 구현해야 한다.
- UserDetails는 User 정보를 관리하는 인터페이스로, `loadUserByUsername` 메서드의 반환 값으로 UserDetails 타입 객체를 반환해야 한다.
- 여기에선 UserDetails 인터페이스를 CustomUserDetails 클래스로 구현했다.

``` java
@Component
public class CustomUserDetailsService implements UserDetailsService {
    private final MemberRepository memberRepository;
    private final CustomAuthorityUtils authorityUtils;

    public CumstomUserDetailsService(MemberRepository memberRepository, CustomAuthorityUtils authorityUtils) {
        this.memberRepository = memberRepository;
        this.authorityUtils = authorityUtils;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Optional<Member> optionalMember = memberRepository.findByEmail(username);
        Member findMember = optionalMember.orElseThrow(() -> new BusinessLogicException(ExceptionCode.MEMBER_NOT_FOUND));

        return new CustomUserDetails(findMember);  // (1) UserDetails 타입(하위)으로 리턴
    }

    // (2) CustomUserDetails 클래스 - User 정보를 관리.
    private final class CustomUserDetails extends Member implements UserDetails {

        CustomUserDetails(Member member) {
            setMemberId(member.getMemberId());
            setFullName(member.getFullName());
            setEmail(member.getEmail());
            setPassword(member.getPassword());
            setRoles(member.getRoles());
        }

        @Override
        public Collection<? extends GrantedAuthority> getAuthorities() {
            return authorityUtils.createAuthorities(this.getRoles());
        }
        
        ...
    }
}
```

- 1,2 과정을 통해 Member 객체에 `PasswordEncoder`를 통한 password 암호화, `CustomAuthorityUtils`를 통한 역할 설정, 권한 설정 데이터를 넣어 DB에 저장할 수 있다.
- 3 과정을 통해 Spring Security에서 CustomUserDetails에 대한 사용자 인증 절차를 수행할 수 있다.
