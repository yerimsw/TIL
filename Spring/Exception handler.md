### 1. Controller 예외처리

- API 계층에서 예외가 발생했을 때 처리 방법을 알아보자.
- `@ExceptionHandler` 애너테이션을 컨트롤러의 핸들러 메서드에 추가해 예외처리를 할 수 있다.
<br>

``` java
@ExceptionHandler
public ResponseEntity handleException(MethodArgumentNotValidException e) {
    final List<FieldError> fieldErrors = e.getBindingResult().getFieldErrors();
    return new ResponseEntity<>(fieldErrors, HttpStatus.BAD_REQUEST);
}
```

1. DTO 클래스 유효성 검증을 실패할 경우 `MethodArgumentNotValidException`이 발생한다.
2. Controller의 `@ExceptionHandler` 애너테이션이 붙은 핸들러 메서드가 던져진 예외를 잡는다.
3. 핸들러 메서드 내에서 e.getBindingResult().getFieldErrors() 메서드를 통해 에러 정보를 확인할 수 있다.
4. 위 에러 정보를 클라이언트에게 ResponseEntity를 통해 Response Body로 전달한다.
<br>

- @ExceptionHandler의 단점
   1. 각 Controller 클래스 마다 @ExceptionHandler 애너테이션이 붙은 예외처리 핸들러 메서드를 중복 작성해야 한다.
   2. Controller에서 발생하는 예외의 종류 별로 여러개의 핸들러 메서드를 작성해야 한다.
