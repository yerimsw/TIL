## 목차

1. [DTO란](#1-dto란)
2. [DTO 사용 이유](#2-dto-사용-이유)
3. [DTO 적용하기](#3-dto-적용하기)
<br>

## DTO

### 1. DTO란

> Data Transfer Object

- 데이터 전송 객체
- 계층 간의 데이터 전달을 위한 자바 Bean 이다.
- 로직을 갖지 않는 순수 데이터 객체로, 속성과 속성 접근을 위한 getter, setter 메서드를 갖는다.
- Object 클래스 메서드는 작성할 수 있다.
<br>

### 2. DTO 사용 이유

1. 코드의 간결성

```java
@PostMapping
public ResponseEntity postMethod(@RequestParam("variable1") Integer v1,
                                 @RequestParam("variable2") Integer v2,
                                 @RequestParam("variable3") Integer v3) {
    Map<String, Integer> map = new HashMap<>();
    map.put("variable1", v1);
    map.put("variable2", v2);
    map.put("variable3", v3);
    
    return new ResponseEntity<Map>(map, HttpStatus.CREATED);
}
```

- Request body로 전달되는 JSON 형식의 데이터를 `@RequestParam`을 통해 변수 값을 얻는다.
- 전송되는 데이터의 수가 늘어나면 비효율적인 코드가 된다.
- 이때, DTO 객체를 사용하면 요청 데이터를 하나의 객체로 전달받을 수 있다.
<br>

```java
@PostMapping
public ResponseEntity postMethod(DtoObject dtoObject) {
    return new ResponseEntity<DtoObject>(dtoObject, HttpStatus.CREATED);
}
```

- `postMethod` 핸들러 메서드의 파라미터로 여러개의 `@RequestParam`을 작성하지 않아도 된다.
- return문에서 ResponseEntity 생성자 매개변수로 DTO 객체를 전달한다.
- 코드가 매우 간결해진다.
<br>

2. 간단한 유효성 검증

- DTO 객체를 사용하지 않으면, 컨트롤러에서 유효성 검증 로직을 추가해야 한다. 👈 SRP 위반!
- 컨트롤러는 클라이언트의 요청을 받아 데이터를 전달하는 것이 주 역할이다.
- 이때, 핸들러 메서드의 유효성 검증 로직을 DTO 객체에게로 분리할 수 있다.
<br>

```java
public class DtoObject {
  
    @Email
    private String email;
    
    @NotNull
    private String name;
    
    Getter & Setter
    ...
}
```

```java
@PostMapping
public ResponseEntity postMethod(@Valid DtoObject dtoObject) {
    return new ResponseEntity<DtoObject>(dtoObject, HttpStatus.CREATED);
}
``` 

- 위는 `dtoObject`에 유효성 검증을 적용하는 코드다.
- 핸들러 메서드의 유효성 검증을 적용할 파라미터 앞에 `@Valid` 애노테이션을 작성한다.
- 컨트롤러로부터 DTO 클래스로 유효성 검증 로직을 분리하여 코드가 간결해졌다.
<br>

### 3. DTO 적용하기

```java
@PostMapping
public ResponseEntity postMethod(@Valid @RequestBody DtoObject dtoObject) {
    return new ResponseEntity<>(dtoObject, HttpStatus.CREATED);
}
```

1. DTO 객체 생성
   - DTO 객체 생성 시, Getter 메서드를 반드시 작성해야 한다.
   - Getter 메서드를 작성하지 않으면 Request Body에 해당 멤버 변수 데이터가 포함되지 않는다.
2. 컨트롤러 핸들러 메서드
   - 클라이언트와 서버 간의 전달 데이터 형식이 `JSON`일 때, `@RequestBody` 애노테이션을 추가한다.
   - `@RequestBody` 애너테이션은 JSON 타입의 Request Body를 자바 DTO 객체로 변환시켜준다.
   - JSON 형식으로 데이터를 전송하지 않으면 "Unsupported Media Type" 에러 메시지를 전송한다.
   - `@ResponseBody` 애너테이션을 핸들러메서드 레벨에 적용하거나, 리턴 타입을 `ResponseEntity`로 작성해야 한다.
