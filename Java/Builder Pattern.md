### 1. 빌더 패턴이란?

- 빌더 패턴은 객체의 필드가 너무 많이 존재할 때, Factory 그리고 Abstract Facotry 디자인 패턴이 갖는 문제점을 해결하기 위해 제공된다. 첫째, 클라이언트로부터 전달 받은 데이터의 순서가 팩토리 클래스의 인자 순서와 다를 수 있다. 둘째, 몇몇 매개변수는 선택적일 수 있음에도 팩토리 패턴에서는 optional 매개변수에 대해 NULL을 전송하여 모든 매개변수를 넘겨야 한다.

- 위와 같은 문제들을 필수 매개변수에 대해 생성자를 제공, 선택적 매개변수에 대해 Setter 메서드를 제공하여 해결할 수 있지만, 이러한 접근은 모든 필드가 온전히 set 되기 전까지 객체의 상태가 일관되지 못하다는 문제가 발생한다. 빌더패턴은 이 문제를 "객체를 한 단계 한 단계씩 빌드하여 최종 객체를 리턴함"으로써 해결할 수 있다. 또한 생성자를 통한 객체 생성이 아니기 때문에 인자의 순서를 고려하지 않아도 된다.
<br>

### 2. 빌더 패턴 코드

1.  static nested class를 생성해 모든 외부 클래스의 필드들을 이너클래스(Builder class)에 동일하게 작성한다. 또한 이너클래스의 이름은 naming convention을 따라야 한다. 외부 클래스명이 Bag 라면 빌더 클래스명은 BagBuilder 이다.
2.  Java 빌더 클래스는 필수 필드에 대해 public 생성자를 제공해야 한다.
3.  Java 빌더 클래스는 optional 필드에 대해 setter 메서드를 제공해야 한다. 주의점은 일반적인 setter 메서드와 다르게 void 타입이 아닌, 필드 값이 setting 된 Builder 객체를 리턴해야 한다.
4.  빌더 클래스는 build() 메서드를 통해 모든 필드 값이 setting 된 객체를 최종 리턴한다. 이는 외부 클래스에서 인자로 Builder 클래스를 갖는 private 생성자를 제공해야 한다. private 생성자는 Builder 클래스 필드 값들을 외부 클래스 필드 값에 복사하는 로직을 갖는다.
<br>

``` java
public class Bag {

  private String name;
  private String color;
  private int price;
  private int size;
  
  // (1) Builder 이너 클래스
  public static class BagBuilder {
    private String name;
    private String color;
    private int price;
    private int size;
    
    // (2) 필수 필드에 대해 생성자를 제공한다.
    public BagBuilder(String name, String color) {
      this.name = name;
      this.color = color;
    }
    
    // (3) Optional 필드에 대해 setter를 제공한다.
    public BagBuilder setPrice(int price) {
      this.price = price;
      return this;
    }
    
    public BagBuilder setSize(int size) {
      this.size = size;
      return this;
    }
  
    // (4) build() 메서드
    public BagBuilder build() {
      return new Bag(this);
    }
  }

  // (4) 외부클래스의 생성자
  public Bag(BagBuilder builder) {
    this.name = builder.name;
    this.color = builder.color;
    this.price = builder.price;
    this.size = biulder.size;
  }
}

Bag bag = new BagBuilder("bagName","red")
         .setSize(1)
         .setPrice(100000)
         .build();
```
<br>

### 3. Lombok @Builder

- 위 코드처럼 일일이 빌더 클래스를 직접 작성하지 않을 수 있다.
- Lombok의 @Builder 애너테이션을 클래스 또는 생성자 위에 붙여주면 된다. (단, 생성자의 경우 생성자에 포함된 필드에 대해서만 적용된다.)
- @Builder.Default 애너테이션으로 특정 변수에 기본 값을 설정할 수도 있다.

```java
import lombok.Builder;

@Builder
public class Bag {
  private String name;
  private String color;
  @Builder.Default private int price = 80000;
  private int size;
}

Bag bag = Bag.builder()
        .name("bagName")
        .color("red")
        .size(1)
        .build();
// price 필드를 지정하지 않아도 price = 80000 기본 값으로 적용되어 있다.
```
