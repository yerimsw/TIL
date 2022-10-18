# Spring Container

## 목차
1. [스프링 컨테이너란](#1.-스프링-컨테이너란)
   1. 상속관계
   2. 빈 메타정보

2. [설정파일](#2.-설정파일)
   1. Java 설정파일
   2. XML 설정파일

3. [빈 조회하기](#3.-빈-조회하기)
<br/>

## 1. 스프링 컨테이너란

- DI 컨테이너, IOC 컨테이너라고 불리기도 한다.
- 스프링 컨테이너는 스프링 빈(객체)의 생성, 의존관계 설정, 소멸 등 빈을 관리한다.
<br/>

### 1) 상속 관계

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20220221172216/Hi.jpg" width="650"/>

> https://www.geeksforgeeks.org/spring-difference-between-beanfactory-and-applicationcontext/

- `ApplicationContext` 인터페이스는 `BeanFactory` 인터페이스를 상속한다.
- `BeanFactory`는 기본적인 빈 관리 기능을 포함하고, `ApplicationContext`는 그 외 부가 기능을 포함한다.
- 스프링 컨테이너로 `ApplicationContext` 인터페이스의 구체 클래스 `AnnotationConfigApplicationContext`를 주로 사용한다.
<br/>

### 2) 빈 메타정보
> BeanDefinition

- 스프링은 다양한 형태의 설정 정보(java, xml, ...)로 부터 `BeanDefinition`을 추상화해 사용한다.
- 컨테이너에 따라 전용 빈 설정파일 리더로 빈 메타정보를 생성한다.
- 애너테이션 기반 자바 설정클래스로 빈을 등록하려면 `AnnotationConfigApplicationContext`의 매개변수로 자바 설정 클래스 파일을 넘겨줘야 한다.
- XML 기반 설정파일로 빈을 등록하려면 `GenericXmlApplicationContext`의 매개변수로 xml 파일을 넘겨줘야 한다.
<br/>

## 2. 설정파일

### 1) Java 설정파일

``` java
@Configuration
public class AppConfig {
  
  @Bean
  public Service service() {
    return new ServiceImpl(repository());
  }

  @Bean
  public Repository repository() {
    return new MemoryRepository();
  }
  
}
```

- `@configuration` `@Bean` 애너테이션을 작성해줘야 한다.
- `@Bean` 애너테이션이 붙은 메서드 명이 빈의 이름이 되고, 호출 시 반환 객체가 빈으로 등록된다.
- 빈 이름은 항상 다른 이름을 부여해야 하고, `@Bean(name="...")` 으로 이름을 직접 부여할 수 있다.
<br/>

### 2) XML 설정파일

``` xml
<bean id="service" class="path.ServiceImpl">
  <constructor-arg name="repository" ref="repository" />
</bean>

<bean id="repository" class="path.MemoryRepository" />
```

- `id`에 빈 이름을 작성하고, `class`에 구현 클래스의 경로를 작성한다.
- `constructor-arg`의 `ref` 속성에 생성자로 주입할 빈 명을 작성한다.
<br/>

## 3. 빈 조회하기

``` java
ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.java);

// 1. 매개변수에 따라 해당하는 빈을 반환한다.
빈타입 참조변수 = ac.getBean("빈 이름");
빈타입 참조변수 = ac.getBean(빈 타입.class);
빈타입 참조변수 = ac.getBean("빈 이름", 빈 타입.class);

// 2. 스프링 컨테이너에 등록된 모든 빈의 이름을 문자열 배열로 반환한다.
String[] beanNames = ac.getBeanDefinitionNames();

// 3. 해당하는 타입의 빈을 Map<빈 이름, 빈 타입> 형태로 반환한다.
Map<String, 빈 타입> beansOfType = ac.getBeansOfType(빈 타입.class);
```

- 이름으로 조회할 경우 `NoSuchBeanDefinitionException` 예외가 발생할 수 있다.
- 타입으로 조회할 경우 `BeanNotOfRequiredTypeException`, `NoUniqueBeanDefinitionException` 예외가 발생할 수 있다.
