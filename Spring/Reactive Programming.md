### Reactive Systems

![ReactiveSystems](https://www.reactivemanifesto.org/images/reactive-traits.svg)

1. MEANS - 커뮤니케이션 수단
   1. Message Driven - 리액티브 시스템에서는 **메시지 기반 통신**을 통해 여러 시스템 간 느슨한 결합을 유지한다.
2. FORM - 형성된 리액티브 시스템의 구조적 특성
   1. Elastic - 시스템으로 들어오는 요청량이 적고 많고에 관계 없이 일정한 응답성을 유지하는 것
   2. Resilient - 시스템의 일부분에 장애가 발생하더라도 응답성을 유지하는 것
3. VALUE - 핵심 가치
   1. Responsive - 클라이언트의 요청에 즉각적으로 응답할 수 있어야 함
   2. Maintainable - 클라이언트의 요청에 즉각적인 응답이 지속 가능해야 함
   3. Extensible - 클라이언트의 요청에 대한 처리량을 자동으로 확장/축소할 수 있어야 함
<br>

### Reactive Streams란?

> 리액티브 프로그래밍을 위한 표준 사양, 명세.

- 리액티브 스트림즈에서 사양으로 정의된 컴포넌트를 알아보자.
1. Publisher
    
    ```java
    public interface Publisher<T> {
    	public void subscribe(Subscriber<? super T> s);
    }
    ```
    
    - `Publisher` 인터페이스는 데이터 소스로부터 데이터를 내보내는 emit 역할을 한다.
    - `Publisher` 인터페이스는 `subscribe()` 추상 메서드를 포함하는데, 메서드 파라미터로 전달되는 `Subscriber`가 emit된 데이터를 소비한다.
<br>

2. Subscriber
    - Subscriber 인터페이스는 **Publisher가 emit한 데이터를 소비**하는 역할을 한다. 총 네 개의 추상 메서드를 포함하며, 각 메서드의 역할을 아래와 같다.
    
    ```java
    public interface Subscriber<T> {
    	public void onSubscribe(Subscription s);
    	public void onNext(T t);
    	public void onError(Throwable t);
    	public void onComplete();
    }
    ```
    
    1. `onSubscribe(Subscription s)`
        
          구독이 시작되는 시점에 호출된다. onSubscrie() 내에서 Publisher에게 요청할 데이터의 개수를 지정하거나 구독 해지 처리를 할 수 있다.
        
    2. `onNext(T t)`
        
          Publisher가 데이터를 emit할 때 호출하며, emit된 데이터를 전달 받아 소비할 수 있다.
        
    3. `onError(Throwable t)`
        
          Publisher로 부터 emit된 데이터가 Subscriber에게 전달되는 과정에서 에러가 발생할 경우 호출된다.
        
    4. `onComplete()`
        
          Publisher의 데이터 emit이 정상적으로 완료된 후 호출된다. emit 종료 후 처리할 작업을 onComplete() 내에서 수행할 수 잇다.
<br>

3. Subscription
    - Susbscription 인터페이스는 Subscriber의 구독 자체를 표현한 인터페이스다.
    
    ```java
    public interface Subscription {
    	public void request(long n);
    	public void cancel();
    }
    ```
    
    1. request(long n) - Publisher가 emit 하는 데이터의 개수를 요청한다.
    2. cancel() - 구독을 해지한다. 구독해지가 발생하면 Publisher는 더이상 데이터를 emit 하지 않는다.
<br>

4. Processor
    - Processor 인터페이스는 Subscriber 인터페이스와 Publisher 인터페이스를 상속하고 있어 둘의 역할을 동시에 수행할 수 있다. 별도로 구현해야할 추상 메서드는 없다.



> 참조) https://www.reactivemanifesto.org/
