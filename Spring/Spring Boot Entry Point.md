## 목차

1. [@SpringBootApplication](#1-@springbootapplication)
2. [Bootstrap](#2-bootstrap)
<br/>

## Spring Boot Entry Point

### 1. @SpringBootApplication

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
   public static void main(String[] args) {
      SpringApplication.run(DemoApplication.class, args);
   }
}
```

- 스프링부트 기반 앱이 실행될 수 있도록 엔트리포인트를 작성해야 한다.
- 스프링부트 엔트리포인트(시작점) 클래스는 반드시 main() 메서드를 포함해야 한다.
- `@SpringBootAnnotation` 애노테이션은 다음 세개의 애노테이션을 포함한다.

  1. @SpringBootConfiguration
  2. @ComponentScan
  3. @EnableAutoConfiguration
<br/>

### 2. Bootstrap

- SpringApplication.run(클래스명.class, args);
- Spring 앱을 bootstrap한 뒤 실행시킨다.
- (+ Bootstrap 과정 추가 예정)
