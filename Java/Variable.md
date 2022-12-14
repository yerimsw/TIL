## 목차
1. [](#)
2. [변수의 초기화](#2-변수의-초기화)
<br>

## 2. 변수의 초기화

### 멤버변수 vs 지역변수

- `멤버변수`: 초기화 불필요, 자동으로 기본 초기화 값이 할당된다.
- `지역변수`: 초기화 필수
<br/>

### 멤버변수 초기화 방법

1. 명시적 초기화

   - 변수의 선언과 동시에 초기화를 진행한다.
   - 가장 간단한 방식이다.

2. 초기화 블럭

   a. 클래스 초기화 블럭
      - 클래스 변수를 초기화 하는데 사용한다.
      - 클래스가 메모리에 로딩되는 시점 한 번만 수행된다.

   b. 인스턴스 초기화 블럭
      - 인스턴스 변수를 초기화 하는데 사용한다.
      - 인스턴스가 생성될 때 수행된다.
      - 생성자 오버로딩 시 공통적으로 구현해야하는 초기화 코드가 있을 때 사용하면 중복을 제거할 수 있다.
  
3. 초기화 순서
   - 클래스 변수 : 기본 초기화 값 -> 명시적 초기화 -> 클래스 초기화 블럭
   - 인스턴스 변수 : 기본 초기화 값 -> 명시적 초기화 -> 인스턴스 초기화 블럭 -> 생성자
