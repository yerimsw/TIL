### 목차
1. [JPA와 영속성 컨텍스트](#1-jpa와-영속성-컨텍스트)
2. [단일 엔티티 테이블 매핑](#2-jpa-단일-엔티티-매핑)
3. [엔티티 간 연관관계 매핑](#3-jpa-엔티티-연관관계-매핑)
<br>

## 1. JPA와 영속성 컨텍스트

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

``` java
@Configuration
public class JpaConfig {
   private EntityManager em;
   private EntityTransaction tx;
   
   @Bean
   public CommandLineRunner testJpaRunner(EntityManagerFactory emFactory) {
      this.em = emFactory.getEntityManeger(); // (1)
      this.tx = em.getTransaction(); // (2)
      
      return args -> {
      
      };
   }
}
```

1. JPA 영속성 컨텍스트를 관리하는 EntityManager 클래스는 EntityManegerFactory 로부터 얻을 수 있다.
2. Transaction 객체는 EntityManager를 통해 얻을 수 있다.
<br>

- 엔티티 저장: em.persist() → INSERT 쿼리
- 엔티티 수정: setter 메서드 → UPDATE 쿼리
- 엔티티 삭제: em.remove() → DELETE 쿼리
- 엔티티 조회: em.find(class, value) → SELECT 쿼리
- 영속성 컨텍스트 변경 사항 테이블에 반영 : tx.commit() 을 통한 em.flush() 호출
<br>

## 2. JPA 단일 엔티티 매핑

### 엔티티와 테이블 매핑
   - `@Entity`: 클래스 레벨에 붙여 엔티티 클래스와 테이블을 매핑한다. JPA 관리 대상 엔티티로 지정한다.
   - `@Table`: 테이블명을 지정한다.
   - `@Id`: 테이블 식별자를 지정한다.

### 기본키 매핑
> @GeneratedValue
   - IDENTITY
   - SEQUENCE
   - TABLE
   - AUTO

### 필드와 칼럼 매핑
   - `@Column`
   - `@Enumerated`
   - `@Temporal`
   - `@Transient`
<br>

## 3. JPA 엔티티 연관관계 매핑

### 연관 관계 정의 기준

- 방향성: 단방향, 양방향
   - 비즈니스 로직에 따라 객체 참조가 한 쪽으로만 이뤄지면 단방향, 서로 간에 객체 참조가 이뤄지면 양방향이다.
   - 기본적으로 단방향 매핑을 한 후, 역방향 객체 참조가 필요할 때 양방향 매핑을 추가하는 것이 좋다.
   - 양방향 연관 관계에서 어떤 객체가 실질적인 권한(저장, 조회, 수정, 삭제)을 갖는지 지정해야 한다.
   - 외래키를 갖는 테이블(외래키 역할 객체를 참조하는 쪽)이 양방향 관계의 제어 권한을 갖는다.

- 참조 객체 수: 일대다, 다대일, 다대다, 일대일
   - 1:N 단방향 연관 관계는 잘 사용하지 않는다.
   - N:1 양방향 연관 관계에서 N 쪽이 외래키를 가지며 제어 권한을 갖는다.
   - 1:1 양방향 연관 관계에서 주 테이블이 외래키를 가지며 제어 권한을 갖는다.
   - N:N 매핑은 외래키 외의 다른 정보를 저장할 수 없고, 중간 테이블에 의해 복잡한 조인 쿼리가 발생할 수 있어 권장하지 않는다.
<br>

### N:1 연관 관계

- 다대일 단방향

```java
@Entity(name = "ORDERS")
public class Order {
   ...
   @ManyToOne // (1)
   @JoinColumn("MEMBER_ID") // (2)
   private Member member;
   ...
}
```
1. 외래키 역할을 하는 객체 참조에 `@ManyToOne` 애너테이션을 작성한다. Order 엔티티와 Member 엔티티가 N:1 관계임을 나타낸다.
2. ORDERS 테이블에서 외래키 칼럼명을 MEMBER_ID로 지정한다. 
<br>

- 다대일 양방향

```java
@Entity
public class Member {
   ...
   @OneToMany(mappedBy="member") // (1)
   private List<Order> orders = new ArrayList<>();
   ...
}
```
1. Order:Member = N:1 이므로 `@OneToMany` 애너테이션을 작성한다.
2. mappedBy에 Order 엔티티에서 Member 객체를 참조하는 필드명을 작성한다.
<br>

### 1:1 연관 관계

- 일대일 단방향

```java
@Entity
public class Post {
   ...
   @OneToOne // (1)
   @JoinColumn("FILE_ID") // (2)
   private File file;
   ...
}
```
1. `@OneToOne` 애너테이션으로 일대일 연관 관계를 나타낸다.
2. `@JoinColumn` 애너테이션으로 외래키의 칼럼명을 지정한다.
<br>

- 일대일 양방향

```java
@Entity
public class File {
   ...
   @OneToOne(mappedBy="file") // (1)
   private Post post;
   ...
}
```
1. `@OneToOne`의 mappedBy에 주 테이블(POST)의 외래키 역할 객체 필드명을 작성한다.
