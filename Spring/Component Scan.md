## 목차

1. [컴포넌트 스캔 사용 이유](#1-컴포넌트-스캔-사용-이유)
2. [컴포넌트 스캔 사용 방법](#2-컴포넌트-스캔-사용-방법)
3. [컴포넌트 스캔 시작 위치](#3-컴포넌트-스캔-시작-위치)
4. [필터](#4-필터)
<br>

### 1. 컴포넌트 스캔 사용 이유

- 자바 설정파일의 경우 `@Bean`을, XML 설정파일의 경우 `<bean>`을 통해 컨테이너에 등록할 빈을 직접 명시했다.
- 등록할 빈의 개수가 늘어나게 되면 설정정보 파일 크기가 커질 뿐더러 매우 번거로운 작업이 된다.
- 이때 컴포넌트 스캔을 도입하면 빈 설정정보 파일을 따로 작성하지 않고 애플리케이션 코드만으로도 스프링 컨테이너를 통한 빈 관리를 할 수 있다.
<br>

### 2. 컴포넌트 스캔 사용 방법

```java
@ComponentScan
public class AppConfig {

}
```

- `@ComponentScan`을 설정 정보 클래스에 작성하면 내부에 빈 정보를 작성하지 않아도 된다.
- 즉 `@Bean` 애너테이션이 붙어 객체를 반환하는 메서드와, 의존 정보를 따로 작성하지 않아도 된다.
- `@ComponentScan`은 `@Component`가 붙은 클래스를 스캔하여 스프링 빈으로 등록한다.
    - 스프링 빈의 이름으로 클래스명의 맨 앞글자를 소문자로 바꾸어 저장한다.
    - `@Component("이름")`을 통해 빈 이름을 직접 설정할 수도 있다.
<br>

```java
@Controller
public class DemoController {
    private final DemoService demoService;
    
    @Autowired
    public DemoController(DemoService demoService) {
        this.demoService = demoService;
    }
}
```
``` java
@Service
public class DemoServiceImpl implements DemoService {
    private final DemoRepository demoRepository;
    
    @Autowired
    public DemoServiceImpl(DemoRepository demoRepository) {
        this.demoRepository = demoRepository;
    }
}
```

- 더이상 설정정보에 의존관계를 명시하지 않으므로 `@Component`가 붙은 클래스를 객체로 생성할 때 의존관계 설정과 주입을 끝마쳐야 한다.
- `@Autowired`는 의존관계를 알아서 설정해 주입해준다.
    - 스프링 컨테이너에 등록 된 빈 중 생성자 매개변수의 타입과 동일한 타입의 빈을 찾아서 주입한다.
    - 생성자 매개변수가 여러개일 때에도 동일하게 자동으로 주입한다.
<br>

### 3. 컴포넌트 스캔 시작 위치

```java
@ComponentScan(basePackages = "시작 위치")
```

- `basePackages` 애너테이션으로 컴포넌트 스캔의 시작 패키지 위치를 지정할 수 있다.
- 컴포넌트 스캔은 지정 시작위치를 포함해 하위 패키지를 모두 탐색한다.
- 디폴트 시작위치 값은 `@Component` 애너테이션이 붙은 설정정보 클래스의 패키지다.
- 설정 정보 클래스의 위치를 프로젝트의 최상단(시작 루트 위치)에 두는 것이 권장된다.
<br>

### 4. 필터

```java
@ComponentScan( 
    includeFilters = @Filter(type = FilterType.ANNOTATION, classes = Include.class),
    excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Exclude.class)
)
```

- @Include 애너테이션이 붙은 클래스는 빈 등록 대상에 포함하고
- @Exclude 애너테이션이 붙은 클래스는 빈 등록 대상에서 제외된다.
