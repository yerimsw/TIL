## 목차

1. [Assertions](#1-assertions)
2. [Assumptions](#2-assumptions)
3. [Lifecycle Method](#3-lifecycle-method)
4. [Hamcrest](#4-hamcrest)
<br>

### 1. Assertions

- 모든 JUnit Jupiter Assertions 클래스 내 메서드는 static 메서드다.
- AssertJ, Hamcrest, Truth 등 third-party assertion 라이브러리를 제공한다.

``` java
import static org.junit.jupiter.api.Assertions.*;

public class Test {
  @DisplayName("assertEquals Test") // (1)
  @Test
  public void assertEqualsTest() {
    String expected = "Junit";
    String actual = "Junit";
    assertEquals(expected, actual); // (2)
  }
  
  @DisplayName("assertNotNull")
  @Test
  public void assertNotNullTest() {
    String actual = null;
    assertNotNull(actual, "Should not be null"); // (3)
  }
  
  @DisplayName("assertionThrows")
  @Test
  public void assertionThrowsTest() {
    assertionThrows(ArithmeticException.class, () -> divide(1,0)); // (4)
  }
  
  private int divide(int num1, int num2) {
    return num1 / num2;
  }
}
```
1. `@DisplayName`으로 테스트 디스플레이 이름을 지정할 수 있다.
2. `assertEquals()` 메서드는 두 객체가 같은 값을 갖는지 검증한다.
3. `assertNotNull()` 메서드는 대상 객체가 null인지 여부를 검증한다.
4. `assertThrows()` 메서드는 대상 메서드를 실행하는 도중 지정 타입 예외가 발생하는지 검증한다.
<br>

### 2. Assumptions

- 모든 JUnit Jupiter Assertions 클래스 내 메서드는 static 메서드다.
- 특정 조건에서만 테스트 로직이 실행되도록 할 수 있다.

``` java
 @Test
 public void assumptionTest() {
  assumeTrue(조건); // (1)
  실행 로직  
}
```
1. `assumeTrue()` 메서드 매개변수로 주어진 값이 true이면 아래의 로직들을 수행한다.
<br>

### 3. Lifecycle Method

> @BeforeAll, @AfterAll, @BeforeEach, @AfterEach 애너테이션이 붙은 메서드를 lifecycle 메서드라고 한다.

- @BeforeAll : 모든 @Test 메서드가 실행되기 전에 @BeforeAll 애너테이션이 붙은 메서드를 한 번 수행한다. 반드시 static 메서드여야 한다.
- @AfterAll : 모든 @Test 메서드가 수행되고 난 후 @AfterAll 애너테이션이 붙은 메서드를 한 번 수행한다. 반드시 static 메서드여야 한다.
- @BeforeEach: 각 @Test 메서드가 실행되기 전 @BeforeEach 메서드를 실행한다.
- @AfterEach : 각 @Test 메서드가 실행된 후 @AfterEach 메서드를 실행한다.
<br>

### 4. Hamcrest

> JUnit 기반 테스트에서 사용할 수 있는 Assertion 프레임워크다.

- Hamcrest 사용 이유

   - Assertion 매처가 하나의 자연스로운 영어 문장으로 이어져 가독성이 향상된다.
   - 테스트 실패 메시지가 기존 JUnit 보다 이해하기 쉽다.
   - 다양한 Matcher를 제공한다.
<br>

- Hamcrest 적용하기

   i. AssertThat
   
   ```java
   @Test
   public void assertionTest() {
     String expected = "Hi";
     String actual = "Hi";
     assertThat(actual, is(equalTo(expected))); // (1)
   }
   ```
   - 기존의 assertEquals(expected, actual) 보다 가독성이 좋다.
   - assert that actual is equal to expected 라는 하나의 영어 문장으로 자연스럽게 이어진다.
  <br>
 
  ii. NotNullValue
  
  ```java
  assertThat(object, is(NotNullValue()));
  assertThat(object, is(NullValue()));
  ```
  - 기존의 assertNotNull(객체, "오류 메시지") 보다 직관적이다.
  - assert that object is not null value 라는 하나의 영어 문장으로 자연스럽게 이어진다.
