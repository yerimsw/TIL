## 목차
1. [@PathVariable](#1-pathvariable)
2. [정규 표현식](#2-정규-표현식)
3. [Custom Validator](#3-custom-validator)
<br>

### 3. Custom Validator

1. Custom Annotation을 정의한다.

``` java
import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = {CustomValidator.class}) // (1)
public @interface AnnotationName { // (2)
    String message() default "디폴트 메시지"; // (3)
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```
        
1. customValidator의 클래스 명을 작성한다.
2. 유효성 검증 애너테이션의 이름을 작성한다.
3. 유효성 검증 실패 시 표시할 디폴트 메시지를 작성한다.
<br>

### 2. CustomValidator 클래스를 구현한다.

```java
import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

public class NotSpaceValidator implements ConstraintValidator<AnnotationName, Type> { // (1)

    @Override
    public void initialize(AnnotationName constraintAnnotation) {
        ConstraintValidator.super.initialize(constraintAnnotation);
    }

    @Override
    public boolean isValid(Type value, ConstraintValidatorContext context) {
        return 유효성 검증 로직에 따른 true or false; // (2)
    }
}
```
1. ConstraintValidator 인터페이스를 구현해야 한다.
   - AnnotationName: CustomValidator와 매핑되는 애너테이션
   - Type: 유효성 검증을 적용할 멤버 변수의 타입

2. 유효성 검증 로직은 isValid 메서드 내부에 작성하여 조건에 따라 true or false를 리턴한다.
