## 목차
> 참조 https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

1. [변수의 타입](#변수의-타입)
2. [값 할당](#값-할당)
<br/>

## 변수의 타입

### 기본형

> Primitive type

- 크기가 작고 고정적이어서 스택에 저장된다.
- 실제 값이 저장되며 null을 저장할 수 없다.
- 복사 시 실제 값이 복사되어 완전히 다른 변수가 생성된다.
- 초기화를 진행하지 않을 시 자동으로 기본 값으로 설정된다.
<br/>

### 기본형 초기화 값

| 원시타입 | boolean | char | byte | short | int | long | float | double |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 초기화 값 | false | '\u000' | 0 | 0 | 0 | 0L | 0.0f | 0.0d|
<br/>

### 참조형

> Reference type

- 크기가 크고 가변적인 힙 메모리에 저장된 객체를 참조한다.
- 객체의 메모리 상 주소값을 저장하며 null값을 저장할 수 있다.
- 복사 시 힙 영역에 존재하는 객체의 메모리 주소를 참조하는 변수가 하나 더 늘어난다.
- 초기화 하지 않을 시 null이 할당되나, `dot` 연산자를 사용하면 `NullPointerException`이 발생함.
<br/>

### 비교 테이블
|  | 기본형 | 참조형 |
| --- | --- | --- |
| 크기 | 작음, 고정적 | 큼, 가변적 |
| 저장 값 | 실제 값, null 불가 | 객체의 메모리상 주소, null 가능 |
| 복사 | 실제 값이 복사 되어 완전히 다른 변수가 생성됨. | 힙 영역에 존재하는 객체를 참조하는 대상이 하나 더 늘어날 뿐임. |
| 초기화 | 자동으로 기본 값이 들어감. | 기본적으로 null 할당, dot 연산자를 사용하면 NullPointerException 발생. |
<br/>

## 값 할당

### int와 long

- 숫자에 l or L을 작성하지 않으면 기본적으로 int 타입이다.

``` java
int n = 1; // n: int type, 1: int type
long n = 1; // n: long type, 1: int type
long n = lL; // n: long type, 1L: long type
```
<br/>

### float과 double

- 소수 값이 있는 숫자에 f or F를 작성하지 않으면 기본적으로 double 타입이다.

``` java
float n = 1.0 // compile error! n: float type, 1.0: double type 
float n = 1.0f;
double n = 1.0d;
```
