## 목차

1. [Spring WebFlux 란?](#1-spring-webflux)
2. [Spring MVC vs Spring WebFlux](#2-spring-mvc-vs-spring-webflux)
3. [구현 코드](#3-구현-코드)
<br>

### 1. Spring WebFlux

- Reactive Stack은 Spring 5 버전 부터 새롭게 추가된 기술 스택이다.
- Spring WebFlux는 Reactive한 웹 애플리케이션을 만들기 위한 웹 프레임워크다.
- Non-blocking 통신, back pressure을 지원하며 Netty, Undertow, Servlet 컨테이너 등에서 동작한다.
<br>

### 2. Spring MVC vs Spring WebFlux

|  | Reactive Stack | Servlet Stack |
| --- | --- | --- |
| 통신 방식 | Non-Blocking | Blocking |
| 라이브러리 | Reactive Streams Adapters | Servlet API에 의존적 |
| 씨큐리티 | Spring Security Reactive | Spring Security |
| API 계층 | Spring WebFlux | Spring MVC |
| DA 계층 | R2DBC, ... | JDBC, JPA, ... |

- Spring은 두가지 기술 스택을 제공한다.
- 하나는 Spring MVC, Spring Data constructs와 함께 Servlet API에 기반한 Servlet Stack이고,
- 하나는 Spring WebFlux와 Spring Data's reactive repositories에 기반한 Reactive Stack이다.
<br>

![spring-mvc-and-webflux-venn](https://user-images.githubusercontent.com/54367532/205072462-de24490f-aa39-48ae-b252-bf4d2bfa9c7c.png)
- Spring MVC와 Spring WebFlux에는 `@Controller` 등 동일하게 적용 가능한 옵션들이 있다.
<br>

### 3. 구현 코드
추가 예정
<br>


> 참조) [Web on Reactive Stack](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html)
