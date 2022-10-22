## 목차

1. [순수 자바 DI 컨테이너](#1-순수-자바-설정파일-컨테이너)
2. [싱글톤 패턴](#2-싱글톤-패턴)
3. [싱글톤 컨테이너](#3-싱글톤-컨테이너)
4. [싱글톤과 상태](#4-싱글톤과-상태)
5. [@Configuration](#5-configuration)
<br/>

### 1. 순수 자바 설정파일 컨테이너

- 요청을 할 때 마다 객체를 생성한다.
- 요청이 많아질 수록 메모리를 낭비하게 된다.
<br/>

### 2. 싱글톤 패턴

* 싱글톤 패턴이란
   - 인스턴스가 단 하나만 생성하도록 보장하는 디자인 패턴이다.
   - 클래스 내부에서 private static final으로 클래스 영역에 객체를 하나만 생성한다.
   - `private` 생성자를 사용해 외부에서 `new` 연산자로 객체를 생성하지 못하게 한다.
   - public `get싱글톤클래스명()` 메서드를 통해서만 접근할 수 있도록 한다.
<br/>

* 장점
   - 요청이 올 때 마다 새로운 객체를 생성하지 않고, 미리 만들어진 객체를 공유한다.

* 단점
   - 싱글톤 패턴을 구현하는 코드가 많다.
   - 빈으로 등록하고자 하는 클래스에 이러한 코드를 모두 작성하기 번거롭다.
   - 클라이언트 코드에서 `싱글톤클래스.get클래스명()` 메서드로 직접 지정하여 사용해야 한다. (구체 클래스 의존: DIP 위반)
<br/>

### 3. 싱글톤 컨테이너

- 스프링 컨테이너는 기본적으로 싱글톤 컨테이너이다.
- 싱글톤 패턴을 구현하는 코드를 따로 작성하지 않아도, 객체를 싱글톤 빈으로 관리한다.
<br/>

### 4. 싱글톤과 상태

``` java
public class SingletonService {
    private int num;
    
    public void setNum(String str, int num) {
        this.num = num;
        System.out.println(str + " : " + num);
    }
    
    public int getNum() { return num; }
}
```
``` java
public class AppConfig {
    
    @Bean
    public SingletonService singletonService() {
        return new SingletonService();
    }
}

@Test
void SingletonTest() {
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.java);
    
    SingletonService singletonService1 = ac.getBean(SingletonService.class);
    SingletonService singletonService1 = ac.getBean(SingletonService.class);
    
    singletonService1.setNum("s1", 1);
    singletonService2.setNum("s2", 2);
    
    int num1 = singletonService1.getNum();
    System.out.println("singletonService1 num : " + num1 + " (expected 1)");
}
```
```
s1 : 1
s2 :2
singletonService1 num : 2 (expected 1)
```

- 싱글톤 객체는 단 하나의 인스턴스이기 때문에 상태를 표시할 멤버 변수가 있으면 문제가 발생한다.
- 공유 객체이기 때문에 여러 메서드에서 객체 참조를 통해 상태를 변경하면 원치 않는 결과 데이터를 반환할 수 있다.
- 따라서 싱글톤 빈은 반드시 상태를 저장할 수 없는 클래스로 정의해야 한다.
<br>

### 5. @Configuration

- Spring은 @Configuration 애너테이션이 붙은 설정파일을 빈으로 등록할 때, CGLID 바이트코드 조작 라이브러리를 사용해 설정파일을 상속한 임의 클래스로 등록한다.
- 해당 클래스는 @Bean 애너테이션이 붙은 메서드에 대해 이미 스프링 컨테이너에 등록된 빈이라면 반환하고, 아니라면 기존의 설정파일을 통해 객체를 생성해 빈으로 등록한다.
- 스프링은 위 과정을 통해 빈의 싱글톤 패턴을 보장한다.
<br>

## 정리

1. 클라이언트 코드에서 직접 new 연산자를 통한 객체를 생성하여 접근했다. -> SRP, DIP, OCP 원칙을 위반
2. 순수 자바 DI 컨테이너(설정파일)는 위 원칙들을 준수했지만 요청마다 새로운 객체를 생성했다.
3. 스프링 컨테이너는 싱글톤 컨테이너로, 객체를 단 하나만 생성하여 공유해 메모리 낭비를 최소화 한다.
4. 싱글톤 빈은 공유 인스턴스이기 때문에 무상태로 정의해야 한다.
5. 스프링은 @Configuration 애너테이션으로 CGLIB 라이브러리를 통해 빈의 싱글톤 패턴을 보장한다.
