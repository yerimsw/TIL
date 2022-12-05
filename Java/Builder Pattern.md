### 빌더패턴

- 빌더패턴이란 이너 클래스 객체를 통해 필드 값을 미리 지정한 후, 외부 클래스 생성자에 이너클래스 객체를 전달하여 객체를 생성하는 방식이다.
- 객체가 생성되면서 필드 값이 설정되기 때문에 이후 객체의 변경이 불가능하다. (immutable 하다.)
- build() 메서드를 통해 필드의 null 여부를 체크할 수 있다.

``` java
public class Bag {

  private final String name;
  private final String color;
  private final int price;
  
  // 객체를 생성하기 전 값을 지정할 Builder 이너 클래스
  public static class Builder {
    private String name;
    private String color;
    private final int price;
    
    // 클래스 필드 별로 각각 값을 지정하는 메서드를 작성한다.
    public Builder name(String name) {
      this.name = name;
      return this;
    }
    
    public Builder color(String color) {
      this.color = color;
      return this;
    }
    
    public Builder price(int price) {
      this.price = price;
      return this;
    }
  
    // 필드 값 설정이 완료된 이너클래스 객체를 외부 클래스 생성자에게 넘겨주는 메서드다.
    public Bag build() {
      return new Bag(this);
    }
  }

  // 이너 클래스 객체의 값을 본 객체에 set한다.
  public Bag(Builder builder) {
    this.name = builder.name;
    this.color = builder.color;
    this.price = builder.price;
  }
}

Bag bag = new Builder()
         .name("bag1")
         .color("red")
         .price(100000)
         .build();
```
<br>

### Lombok @Builder

- 위 코드처럼 일일이 빌더 클래스를 직접 작성하지 않을 수 있다.
- Lombok의 @Builder 애너테이션을 클래스 또는 생성자 위에 붙여주면 된다. (단, 생성자의 경우 생성자에 포함된 필드에 대해서만 적용된다.)

```java
import lombok.Builder;

@Builder
public class Bag {
  private String name;
  private String color;
  private int price;
}
```
