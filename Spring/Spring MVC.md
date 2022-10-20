## 목차

1. [Spring MVC](#1-spring-mvc)
2. [DispatcherServlet](#2-dispatcherservlet)
<br/>

## 1. Spring MVC

### Spring MVC란

> https://www.javatpoint.com/spring-mvc-tutorial

![image](https://user-images.githubusercontent.com/54367532/196936949-bd98f26c-324a-429e-887a-8adcb6ec12f6.png)

- Servlet API를 기반으로 클라이언트의 요청을 처리하는 스프링 웹 프레임워크다.

### Model

- `Model`은 요청에 대한 응답으로 돌려주는 결과 데이터다.
- 클라이언트의 요청을 처리하는 코드를 비즈니스 로직이라고 하며, 이 로직을 수행하는 영역을 서비스 계층이라고 한다.
- Map 자료구조로, HTTP 요청 데이터를 key value 로 저장한다.

### View

- `View`는 Model 데이터를 특정 포맷으로 변환하여 클라이언트에게 제공한다.
- Spring은 다양한 View 기술들(HTML, PDF, XML, JSON 등)을 제공한다.

### Controller

- `Controller`는 클라이언트의 요청을 받아 Model과 View의 중간에서 데이터를 전달한다.
- 즉, 클라이언트의 요청을 전달 받고, 비즈니스 로직을 통해 Model객체를 전달받아 View에 전달한다.
- `@Controller` 애노테이션은 이 클래스가 Spring MVC Controller임을 나타낸다.

### 정리

- Client가 요청 데이터를 전송
- Controller가 요청 데이터를 수신하여 Model 데이터를 반환
- Model 데이터를 View에게 전달
- View가 응답 데이터를 생성하여 Client에게 전달
<br/>

## 2. DispatcherServlet

> https://www.javatpoint.com/spring-mvc-tutorial

![image](https://user-images.githubusercontent.com/54367532/196936979-79db369b-a5de-4044-a30e-aea3e6e0ae6a.png)

- Spring MVC 앱에서 `DispatcherServlet` 클래스는 클라이언트의 요청을 받아 앱 흐름을 관리한다.
- 컨트롤러, 뷰 등에 요청과 응답을 전달해주는 DispatcherServlet을 `Front Controller` 라고 한다.

### Spring MVC 동작 방식
1. 클라이언트로부터 요청이 들어온다.
2. DispatcherServlet은 HandlerMapping에 요청을 전달하고 적합한 컨트롤러 정보를 전달 받는다.
3. DispatcherServlet은 해당 Controller를 호출한다. (정확히는 HandlerAdapter에게 Controller 호출 권한을 위임하여 진행된다.)
4. Controller로 부터 Model객체와 viewName을 전달받는다.
5. viewResolver는 ViewName을 통해 적절한 View를 컨트롤러에게 리턴한다.
6. View에 Model 데이터를 전달해 알맞은 응답 데이터를 생성해 클라이언트에게 전달한다. 
