## Spring Data JDBC에서 페이징 인터페이스 사용하기
![](Spring/JDBC_Pagination.png)

### CrudRepository 인터페이스

- CrudRepository 인터페이스의 CRUD 쿼리 메서드에 PageRequest를 매개변수로 넘길 수 있다.
- Page< T> 타입 객체가 반환된다.
- [PageRequset](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/PageRequest.html) 클래스는 [Pageable](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/Pageable.html) 인터페이스를 구현한 AbstractPageRequest를 상속한다.
- PageRequest 클래스는 page, size, sort 멤버 변수를 갖는다.
<br>

### Page<T>

| Method | Type | Description |
| --- | --- | --- |
| getTotalElements() | long | 전체 원소의 개수를 반환한다. |
| getTotalPages() | int | 전체 페이지 수를 리턴한다. |

- 전체 요소 리스트 중 페이지(서브 리스트)에 대한 정보를 제공한다.
<br>

### Slice<T>

| Method | Type | Description |
| --- | --- | --- |
| getContent() | List<T> | 페이지 컨텐츠를 리스트 타입으로 반환한다. |

- Page<T> 인터페이스가 상속하는 인터페이스다.
<br>
