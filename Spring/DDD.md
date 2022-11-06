## DDD

> Domain Driven Design 도메인 주도 설계
<br>

### Aggreagte

- 하나의 트랜잭션 내에서 여러 객체의 변경이 있을 때 애그리거트 사용을 고려해야 한다.
- 애그리거트는 개념적으로 연관된 엔티티들의 캡슐이다.
- 일관성을 유지하기 위해 애그리거트에 적용되는 규칙(invariants)이 있을 수 있다.
- 또한 애그리거트에 속한 전체 엔티티를 함께 DB에 저장하거나 DB로부터 얻는다.
<br>

### Aggregate Root

- 애그리거트 루트는 entry point로써 동작한다.
- 애그리거트의 일관성 유지를 위해 모든 비즈니스 연산은 애그리거트 루트를 통해서 수행할 수 있다.
- 한 애그리거트에 속한 객체들은 루트 엔티티에 직간접적으로 속한다.
<br>

### 애그리거트 객체 테이블 매핑 규칙

1. 모든 엔티티 객체의 상태는 애그리거트 루트를 통해서만 변경할 수 있다.
2. 동일 애그리거트 내 엔티티간 관계는 객체간 참조로 매핑한다.
3. 다른 애그리거트간 관계는 ID 참조로 매핑한다.
<br>

> Aggregates are the basic element of transfer of data storage - you request to load or save whole aggregates.
> Transactions should not cross aggregate boundaries.

— Martin Fowler
