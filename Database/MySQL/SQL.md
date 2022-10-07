## MySQL SQL 문법
> 아직 덜 익숙한 문법들을 위주로 모았다.

### WHERE

- 조건에 해당되는 레코드를 필터링 한다.
- 조건에서 텍스트 필드는 `‘` 또는 `“` 으로 감싸지만, 숫자 필드는 없어야 한다.

``` sql
SELECT 칼럼1, 칼럼2 FROM 테이블 WHERE 조건
UPDATE ~~
DELETE ~~
```

| Operator | Description |
| --- | --- |
| <> | 주어진 값과 칼럼 값이 동일하지 않은 레코드 |
| BETWEEN | 특정 범위 내의 칼럼 값을 갖는 레코드 |
| LIKE | 특정 패턴을 갖는 칼럼 값을 갖는 레코드 |
| IN | 특정 값들 중 해당하는 칼럼을 갖는 레코드 |
<br/>

### UPDATE

- 레코드의 값을 수정한다.

```sql
UPDATE 테이블명
SET 칼럼1 = 값1, 칼럼2 = 값2, ...
WHERE 조건;
```
<br/>

### DELETE

- 조건에 해당되는 레코드를 삭제한다.

```sql
DELETE FROM 테이블 WHERE 조건;
```
<br/>

### INSERT INTO

- 테이블에 새로운 레코드를 추가한다.

```sql
-- 특정 칼럼에만 값을 삽입하는 경우 -> 정해지지 않은 칼럼은 디폴트 값으로 올라간다.
INSERT INTO 테이블 (칼럼1, 칼럼2)
VALUES (값1, 값2);

-- 모든 칼럼에 값을 삽입하는 경우
INSERT INTO 테이블 VALUES (값1, 값2, ...);
```
<br/>

### CASE

- `if else`문 처럼 동작한다.
- 조건에 따라 값을 달리할 수 있다.

```sql
SELECT 칼럼1,
CASE
	WHEN 조건1 THEN 값1,
	WHEN 조건2 THEN 값2,
	ELSE 값3
END AS 나타낼 칼럼명
FROM 테이블
```
<br/>

### COUNT, AVG, SUM

- 레코드의 수, 평균, 합계를 반환한다.

```sql
SELECT COUNT(칼럼) / AVG(칼럼) / SUM(칼럼)
FROM 테이블
WHERE 조건;
```
<br/>

### GROUP BY

- 특정 칼럼을 기준으로 동일한 값을 갖는 레코드 끼리 그룹화 한다.
- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` 등 집계함수와 자주 사용된다.

```sql
SELECT 칼럼 FROM 테이블 WHERE 조건
GROUP BY 칼럼;
```
<br/>

### HAVING

- WHERE 절은 집계함수를 사용할 수 없다.
- 따라서 집계함수를 조건으로 사용하기 위해 사용한다.

```sql
SELECT 칼럼 FROM 테이블
WHERE 조건
GROUP BY 칼럼
HAVING 조건;
```
