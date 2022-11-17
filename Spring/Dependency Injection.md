## 목차

1. [의존관계 주입 방법](#1-의존관계-주입-방법)
   - 생성자 주입
   - 세터 주입
   - 필드 주입
   - 메서드 주입

2. [의존관계 옵션 처리](#2-optional-dependency)
3. [동일 타입 빈 조회](#3-동일-타입-빈-조회)
4. [동일 타입 모든 빈 조회](#4-동일-타입-모든-빈-조회)
<br>

## 1. 의존관계 주입 방법

> 스프링 컨테이너가 관리하는 빈에서 의존관계를 주입하는 여러 방법이 있다.

### 1. 생성자 주입

``` java
@Service
public class DemoService {
  private final DemoRepository demoRepository;
  
  // @Autowired - 생성자가 유일하다면 생략할 수 있다.
  public DemoService(DemoRepository demoRepository) {
    this.demoRepository = demoRepository;
  }
  
  ...
}
```

- 생성자를 통해서 의존관계를 주입 받는다.
- 일반적으로 여러 의존관계 주입 방법 중 생성자 주입을 권장한다.
- 그 이유는 다음과 같다.
<br>

1. 변하지 않음
   - 애플리케이션 의존관계의 대부분은 종료되기 까지 변하지 않는다.
   - 생성자 주입의 경우 생성자를 통해서 의존 관계가 성립 된다.
   - 생성자는 객체가 생성될 때 한 번만 호출하므로 의존관계가 변하지 않는다.

2. final 키워드
   - 의존관계가 변하지 않으므로 의존하는 객체 필드를 선언할 때 final 키워드를 사용할 수 있다.
   - final 키워드를 사용하기 때문에 동시에 필드 초기화 작업이 이뤄져야 한다. (필수)
<br>

### 2. Setter 주입

``` java
@Service
public class DemoService {
  private DemoRepository demoRepository;
  
  @Autowired
  public void setDemoRepository(DemoRepository demoRepository) {
    this.demoRepository = demoRepository;
  }
  
  ...
}
```

- Setter 메서드를 통해 의존관계를 주입한다.
- 주로 변경 가능성이 있는 의존관계에 사용한다.
<br>

### 3. 필드 주입

``` java
@Service
public class DemoService {
  @Autowired
  private DemoRepository demoRepository;
  ...
}
```

- 필드에 @Autowired 애너테이션만 붙여주면 되어 간결하지만 권장되지 않는다.
- DI 프레임워크에 의해서만 동작하고 외부에서 변경이 불가능하다는 단점이 있다.
<br>

### 4. 메서드 주입

``` java
@Service
public class DemoService {
  private DemoRepository demoRepository;
  
  @Autowired
  public void init(DemoRepository demoRepository) {
    this.demoRepository = demoRepository;
  }
  
  ...
}
```

- 일반적으로 잘 사용하지 않는다.
- setter 주입의 경우 한 필드씩 의존관계를 주입받지만, 메서드 주입은 한 번에 여러 필드를 주입받을 수 있다.
<br>

## 2. Optional Dependency

> 주입할 스프링 빈이 없을 때 의존 주입을 옵션으로 처리할 수 있는 방법이 있다.

- `Autowired(required = false)` : 주입할 대상이 없으면 setter 메서드를 호출하지 않는다.
- `@Nullable` : setter 주입에서 자동 주입할 대상이 없으면 null을 주입한다.
- `Optional<>` : setter 주입에서 자동 주입할 대상이 없으면 Optional.empty를 주입한다.
<br>

## 3. 동일 타입 빈 조회

### 1. 해결 방법

- 의존성 주입의 `@Autowired`를 통해 스프링은 빈을 타입으로 조회한다.
- 이때 타입이 같은 빈이 두개 이상 조회되면 `NoUniqueBeanDefinitionException` 예외가 발생한다.
- 이를 해결하기 위한 세가지 방법을 알아보자.
<br>

1. 빈 이름 매칭
   - 타입 매칭 이후 여러개의 빈이 조회되었을 때, 필드명 -> 매개변수명 순으로 이름이 일치하는 빈을 조회한다.

2. Qualifier
   - `@Component` 애너테이션이 붙은(빈으로 등록하기 위해) 클래스에 `@Qualifier("빈 명")`을 작성한다.
   - 생성자, 세터, 필드 주입 위치에 `@Qualifier("빈 명")`을 동일하게 작성하면 일치하는 Qualifier 빈을 매칭한다.
   - 일치하는 Quailifier가 없으면 이름이 동일한 빈을 매칭한다.

3. Primary
   - 동일 타입 빈들 중 @Primary가 붙은 빈이 매칭 우선권을 갖는다.
   - Qualifier은 파라미터나 필드에 애너테이션을 일일이 작성해야하지만, Primary는 빈에 한 번만 작성하면 된다는 장점이 있다.
   - Qualifier과 Primary 중 매칭 우선권은 수동으로 동작하는 Quailifer가 갖는다.
<br>

### 2. 애노테이션 생성

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER,
ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Qualifier("mainPolicy")
public @interface MainPolicy {
   ...
}
```

- @Qualifier("문자열 식별자") 를 통해 빈 매칭을 하는 방법은 문제점이 있다.
- 두 애노테이션 간의 문자열이 상이하더라도 문자열이기 때문에 컴파일 타임에 오류를 뱉지 않고 런타임에 매치되는 빈이 없다고 에러가 발생한다.
- 따라서 위와 같이 기존 Qualifier의 기능을 수행하는 애노테이션을 새롭게 생성할 수 있다.
<br>

``` java
@Component
@MainPolicy
public class PolicyOne extends Policy {
   ...
}
```
``` java
public class DemoService {
   private final Policy policy;
   
   @Autowired
   public DemoService(@MainPolicy Policy policy) {
      this.policy = policy;
   }
}
```
- 위와 같이 애노테이션을 빈 객체와 의존주입 하려는 생성자에 적용하여 사용할 수 있다.
- IntelliJ와 같은 개발 환경에서 애노테이션이 어디에 사용되었는지 트래킹도 가능하다.
<br>

## 4. 동일 타입 모든 빈 조회

> 동일한 타입의 빈을 모두 조회하여 어떤 빈을 주입할지 동적으로 정할 수 있다.

```java
@Test
void findAllBean() {
	ApplicationContext ac = new AnnotationConfigApplicationContext(DiscountService.class);
	
} // 스프링 컨테이너

static class DiscountService {
	private final Map<String, DiscountPolicy> policyMap;
	private final List<DiscountPolicy> policies;

	public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
		this.policyMap = policyMap;
		this.policies = policies;
	}

	public int discount(Member member, int price, String DiscountCode) {
		DiscountPolicy discountPolicy = policyMap.get(DiscountCode);
		return discountPolicy.discount(member, price);
}
```

- Map<String, 빈 타입>: 해당 빈 타입을 갖는 모든 빈을 조회하여 `빈 이름 = 빈 타입` 형태로 자동 주입 받는다.
- List<빈 타입>: Map 에서 빈 이름만 빠진 형태다.
- 다형성 코드를 유지하면서 동적으로 빈을 설정해야할 때 매우 유용하다.
