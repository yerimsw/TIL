## 목차
1. [앱 설계](#1-앱-경계-설정--비즈니스-요구사항-수집)
2. [컨트롤러 계층](#2-컨트롤러-계층)
3. [서비스 계층](#3-서비스-계층)
<br>

## 1. 앱 경계 설정 & 비즈니스 요구사항 수집

- 앱 경계 : 어떤 애플리케이션인지 설정한다.
- 비즈니스 요구사항
    - 필요 엔티티: 필요한 리소스는 무엇인가.
    - 비즈니스 로직: 이 앱이 처리해야할 요구사항은 무엇인가.
<br>

## 2. 컨트롤러 계층

### 1. 컨트롤러 분리하기

- Rest API 기반의 앱은 일반적으로 앱이 제공할 기능을 리소스 별로 분류한다.
- 각 리소스에 해당하는 Controller를 작성하면 된다.
<br>    
    
### 2. 엔트리포인트 클래스 작성

- main() 메서드가 포함된 시작점을 작성한다.
- `@SpringBootApplication` : `@ComponentScan` 빈 등록, `@Configuration` 클래스 탐색하여 빈 등록 기능을 활성화한다.
- SpringApplication.run(클래스명.class, args): 스프링 앱을 실행한다.
 <br>
 
### 3. Controller 구조 작성

1. `@RestController` : 이 컨트롤러 클래스가 REST API의 리소스를 처리하는 엔드포인트로 동작함을 의미한다. Spring Bean으로 등록된다.

2. `@RequestMapping("/path")` : URI 경로 요청에 따라 핸들러 메서드를 매핑한다. 보통 Controller 클래스 레벨에 추가해 클래스 전체에서 사용되는 공통 URL(Base URL)을 설정한다.

3. 핸들러메서드 적용
   1. `PostMapping` `GetMapping` `PatchMapping` `DeleteMapping` : CRUD에 해당하는 HTTP 매핑이다.
   2. `@RequestParam("data")` : 요청 JSON 데이터에서 파라미터 값을 가져온다.
   3. `@PathVariable("변수명")` : URI 중 `{변수명}` 값을 받아온다.

4. `ResponseEntity` 응답 객체 적용
   1. ResponseEntity로 리턴하면 JSON 데이터를 응답으로 보낼 수 있다.
   2. return new ResponseEntity(HttpStatus.CREATED);

5. `DTO` 객체 적용
   1. JSON 요청 데이터를 하나의 객체로 받을 수 있고, 유효성 검증을 따로 빼 Controller의 SRP를 유지한다.
   2. HTTP Method 별 DTO 유효성 검증 로직이 다를 수 있으므로 DTO 객체를 따로 생성한다. Ex) `MemberPostDto`, `MemberPatchDto`
   3. DTO 객체에 `@Getter` 가 반드시 있어야 한다.
   4. `@RequestBody` 애너테이션을 DTO 파라미터 앞에 작성해야 한다.
   5. `@ResponseBody` 애너테이션을 사용하면 DTO 클래스를 JSON 형식의 Response Body로 변환한다. 그런데, 핸들러 메서드의 리턴 값이 `ResponseEntity` 일 경우 내부에서 응답 객체를 JSON형식으로 바꿔준다.

6. DTO 유효성 검증
   1. 의존 라이브러리 추가 `implementation 'org.springframework.boot:spring-boot-starter-validation'`
   2. 컨트롤러 핸들러 메서드 파라미터 DTO 앞에 `@Valid` 애너테이션을 추가한다.
   3. DTO 객체에 `@NotBlank` `@Email` `@Pattern` 등 검증 애너테이션을 적용한다.
   4. URI 변수 값 유효성 검증 : 컨트롤러 레벨에 `@Validated` 애너테이션을 추가한다.  `@PathVariable("data")` `@Min(0)` 과 같이 작성한다.
   5. Custom Validation Annotation을 적용할 수 있다.
<br>

## 3. 서비스 계층

### 1. API 계층과 Service 계층의 연동

1. Spring에 의한 DI (의존 주입)
```java
private final MemberService memberService;

@Autowired // 단일 생성자 -> 생략 가능
public MemberController(MemberService memberService) {
    this.memberService = memberService;
}
```
<br>

2. Service 메서드 작성
   1. Controller의 Handler Method와 1대1 매칭된다.
   2. 비즈니스 로직 작성에 따라 컨트롤러의 핸들러 메서드에서 비즈니스 로직을 호출하도록 한다.
<br>

### 2. Entity 클래스

1. Entity 클래스 작성
   1. DTO 클래스 멤버 변수를 모두 포함한다.
   2. `@Getter` `@Setter` `@AllArgsConstructor` `@NoArgsConstructor`
<br>

2. Mapper
   1. Entity 객체에서 응답 DTO 객체로 변환할 수 있도록 `ResponseDto` 클래스를 생성한다.
   2. DTO 객체와 Entity 객체 간 서로 변환될 수 있도록 Mapper 클래스를 구현한다.
   3. MapStruct를 이용해 Mapper를 자동 생성 할 수 있다.
   4. Mapper 클래스 작성에 따른 Controller의 객체 변환 책임이 분리되었으므로 DI로 주입한다.
<br>
