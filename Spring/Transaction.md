## JPA commit

> EntityManager.getTransaction() 으로 얻은 Transaction 객체의 commit() 메서드 호출 시 동작과정

![transaction](https://user-images.githubusercontent.com/54367532/200114172-36ef17a5-fb56-4b9a-b09c-41dda1434cde.png)


1. EntityTransaction.commit()을 호출하면 `EntityTransaction` 인터페이스를 구현한 `TransactionImpl` 클래스의 commit()이 호출된다.
2. 위 commit() 메서드 내부에서 `TransactionDriver` 인터페이스의 commit()을 호출하면, 
3. TransactionDriver 인터페이스를 구현한 `TransactionDriverControlImpl` 클래스의 commit()을 호출한다.
4. 위 commit() 메서드 내부에서 `JdbcResourceTransaction` 인터페이스의 commit()을 호출한다.
5. JdbcResourceTRansaction 인터페이스를 구현한 `AbstractLogicalConnectionImplementor` 클래스의 commit()을 호출한다.
6. 위 commit() 메서드 내부에서 `Connection` 인터페이스의 commit()을 호출한다.
7. Connection 인터페이스를 구현한 `JdbcConnection` 클래스의 commit()을 호출한다.
8. H2에서 지원하는 Command 클래스에 따라 SessionLocal 클래스에서 최종 트랜잭션 commit()이 이뤄진다.
