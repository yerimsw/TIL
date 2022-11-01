### JPA란

> Java Persistence API

- 자바 진영에서 사용하는 ORM(Object-Relational Mapping) 기술의 표준 사양이다.
- JPA 라는 표준 사양은 인터페이스로 정의되고, 이를 구현한 구현체가 따로 존재한다.
- 대표적인 JPA 구현체로 `Hibernate ORM`, `EclipseLink`, `DataNucleus` 등이 있다.
<br>

### 영속성 컨텍스트

- Persistence의 의미는 영속성이다. 
- ORM은 객체와 DB 테이블의 매핑을 통해 엔티티 데이터를 테이블에 저장하는 기술이다.
- `JPA`는 엔티티 객체를 테이블에 매핑하여 영속성 컨텍스트에 보관해 지속한다.
- 영속성 컨텍스트에는 1차 캐시영역, 쓰기지연 SQL 저장소가 존재한다.
<br>

### JPA API

- 엔티티 저장: em.persist() → INSERT 쿼리
- 엔티티 수정: setter 메서드 → UPDATE 쿼리
- 엔티티 삭제: em.remove() → DELETE 쿼리
- 엔티티 조회: em.find(class, value) → SELECT 쿼리
- 영속성 컨텍스트 변경 사항 테이블에 반영 : tx.commit() 을 통한 em.flush() 호출

### JPA 엔티티 매핑

1. 엔티티와 테이블 매핑
2. 기본키 매핑
   - IDENTITY
   - SEQUENCE
3. 필드와 칼럼 매핑
4. 엔티티와 엔티티 연관관계 매핑
