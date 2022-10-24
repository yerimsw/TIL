## 목차

1. [Service layer](#1-service-layer)
2. [Entity](#2-entity)
3. [Mapper](#3-mapper)
<br>

### 1. Service Layer

- API 계층과 데이터 접근 계층 사이에서 비즈니스 로직을 처리한다.
- 컨트롤러의 핸들러 메서드와 서비스의 비즈니스 로직 처리 메서드는 1대 1로 매치 된다.
- 서비스 계층에서는 DTO로부터 데이터를 매핑한 Entity 클래스로 비즈니스 로직을 동작시킨다.
<br>

``` java
@RestController
@RequestMapping("/uri")
public class MemberController {
    private final Service service;

    @Autowired
    public Controller(Service service) {
        this.service = service();
    }
    ...
}
```

- 컨트롤러의 생성자를 통해 서비스 빈을 의존 주입할 수 있다.
- 이를 위해 스프링 컨테이너에 컨트롤러, 서비스가 빈으로 등록 되어있어야 한다.

<br>

### 2. Entity

``` java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Entity {
    private long id;
    private String name;
    private int age;
}
```

- API 계층으로부터 데이터를 받아 비즈니스 로직을 실행 후, 그 결과 값을 다시 API 계층에 리턴한다.
- 따라서 Entity는 DTO의 멤버 변수를 모두 포함한다.
- [Lombok](https://projectlombok.org/features/) 라이브러리를 이용하여 getter, setter, 생성자를 자동 생성할 수 있다.
- DTO와 Entity 클래스를 분리하는 이유
   - DTO는 API 계층에서 클라이언트 요청데이터를 전달하는 것이, Entity는 DAO와 연동해 비즈니스 로직 처리 데이터를 다루는 것이 주 역할이다.
   - DTO 클래스를 활용하면 DB에서 클라이언트 응답으로 원하는 데이터만 제공할 수 있다.
<br>

### 3. Mapper

- API 계층으로부터 Service 계층에서 사용할 데이터를 Entity를 통해 전달 받는다고 했다.
- 이때 DTO 클래스를 Entity 클래스로 변환하거나 Entity 클래스를 다시 DTO 클래스로 변환해주는 Mapper가 필요하다.
- 컨트롤러 핸들러 메서드 내에서 DTO와 Entity 간 변환 역할을 Mapper 클래스에게 위임하는 이유
   - 컨트롤러는 클라이언트 요청 데이터를 서비스에 전달, 응답 데이터를 클라이언트에게 전달하는 역할만을 갖는 것이 바람직하다. (SRP 준수)
   - 서비스 계층에서 비즈니스 로직 처리 결과 데이터로 반환된 Entity 객체를 그대로 클라이언트에게 전송하면 계층 간 역할 분리가 제대로 이루어지지 않는다.
<br>

``` java
public class Mapper {
    public dtoToEntity(DtoObject dtoObject) {
        return new Entity(
            dtoObject.getId(),
            dtoObject.getName(),
            dtoObject.getAge()
        );
    }
    
    public entityToDto(Entity entity) {
        return new DtoObject(
            entity.getId(),
            entity.getName(),
            entity.getAge()
        );
    }
}
```

- 위와 같이 DTO와 Entity 클래스 간 타입 변환이 가능하다.
- 그러나 도메인이 늘어날 때 마다 일일이 매퍼클래스를 작성하는 것은 비효율적이고 에러를 낳기 쉽다.
- [MapStruct](https://mapstruct.org)와 같은 코드 생성기를 이용하는 것이 효율적이다.
<br>

``` java
@Mapper(componentModel = "spring")
public interface Mapper {
    Entity dtoObjectToEntity(DtoObject dtoObject);
    DtoObject entityToDtoObject(Entity entity);
}
```

- `@Mapper(componentModel="spring")` 애너테이션을 적용해 Mapper 구현 클래스를 `spring` 빈으로 등록한다.
- DTO와 Entity 간 변환 추상 메서드를 작성한다.
- MapStruct가 위 인터페이스를 기반으로 구현 클래스를 생성해준다.
