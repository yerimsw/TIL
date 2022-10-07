## Relational Database

### RDB
> Relational Database 👉 관계형 데이터베이스 

- 관계형 DB는 데이터를 행과 열의 2차원 테이블의 형태로 표현한다.
- 사전에 정의된 테이블 구성(schema)에 따라 일치하는 형태의 데이터만을 삽입한다.
- SQL을 사용하여 원하는 데이터를 질의(필터링)를 통해 얻을 수 있다.
- SQL은 MySQL, Oracle, SQLite, PostgreSQL, MariaDB 등이 있다.
<br/>

### 키워드

| keyword | about |
|:---:|---|
| data | 각 항목에 저장되는 값 |
| table / relation | 사전에 정의된 칼럼의 정보에 따라 데이터가 행으로 축적되는 것 |
| column / field | 테이블의 한 열 |
| record / tuple | 테이블의 한 행 |
| key | 테이블의 각 record를 구분짓는 고유한 값 |
<br/>

### Schema

| Field | Type | Null | Key | Extra |
| --- | --- | --- | --- | --- |
| id | int | No | PK | auto_increment |
| name | varchar(255) | No |  |  |
| email | varchar(255) | No |  |  |

- DB 엔터티의 필드, 타입 등 데이터가 구성되는 방식을 나타낸다.
- 서로 다른 엔터티(테이블) 간의 관계를 설명한다.
<br/>

## Relationship

> 각 엔터티는 관계가 존재한다.
<br/>

### 1:1

> 하나의 레코드가 다른 테이블의 하나의 레코드와 연결된 경우

- one to one relationship
- 1:1 관계는 단일 테이블로 나타내는 것이 더 효율적일 수도 있다.
<br/>

### 1:N

> 하나의 레코드가 다른 테이블의 여러개의 레코드와 연결된 경우

- one to many relationship
- 관계형 DB에서 가장 많이 사용하는 관계 유형이다.
- 외래키와 기본키를 사용해 나타낸다. 외래키는 참조하는 테이블의 기본키를 참조한다.
<br/>

### N:N

> 여러개의 레코드가 다른 테이블의 여러개의 레코드와 연결된 경우

- many to many relationship
- n:n 관계는 두 테이블을 참조하는 조인 테이블을 생성해 데이터를 관리한다.
- 각 참조 테이블과 조인 테이블의 관계는 1:N이 성립한다.
<br/>

### 자기참조

> 한 엔터티가 자기 자신과 관계를 맺는 경우

- recursive relationship
- 동일 Entity 내에서 자기가 자신의 인스턴스를 참조하는 구조이다.
- 추천인, 조직 등 계층적으로 구성되는 관계를 나타내는 데 주로 사용된다.
