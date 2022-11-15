## 목차

1. [의존관계 주입 방법](#1-의존관계-주입-방법)
   - 생성자 주입
   - 세터 주입
   - 필드 주입
   - 메서드 주입

2. [의존관계 옵션 처리](#2-optional-dependency)
3. [동일 타입 빈 조회](#3-동일-타입-빈-조회)
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
