### Project Reactor란?

- Reactive Streams 표준 사양을 구현한 라이브러리 중 하나이다.
- Spring5 부터 지원하는 리액티브 스택에 포함되어 있다.
<br>

### 특징

- Reactive Streams를 구현한 리액티브 라이브러리이다.
- 요청 쓰레드가 차단 당하지 않는 Non-Blocking 통신을 지원한다.
- 때문에 서비스 간 통신이 잦은 MSA 기반 애플리케이션에 적합한 라이브러리이다.
- Publisher 타입으로 `Mono[0|1]` `Flux[N]`을 지원한다.
- Subscriber의 처리 속도가 Publisher의 emit 속도를 따라가지 못할 때 데이터를 적절히 제어하는 `Backpressure` 전략을 지원한다.
<br>

### Reactor 구성 요소

``` java
Flux // (1)
    .just("Hello","Reactor") // (2)
    .map(message-> message.toUpperCase())
    .publishOn(Schedulers.parallel())
    .subscribe(System.out::println,
               error -> System.out.println(error.getMessage()),
               ()-> System.out.println("# onComplete"));
```

1. Reactor Sequenece의 시작점, `Flux`로 시작하므로 여러 건의 데이터를 처리한다.
2. `just()` 연산자는 원본 데이터 소스로부터 데이터를 emit 하는 Publisher의 역할을 한다.
3. `map()` operator는 Publisher로부터 전달 받은 데이터를 가공한다. Reactor에서는 map() 외에도 많은 Operator를 제공한다.
4. `publishOn()`은 쓰레드를 관리하는 `Scheduler`를 지정한다. Downstream의 쓰레드가 지정한 쓰레드로 변경된다.
5. `subscribe()` 는 파라미터로 총 세개의 람다 표현식을 갖는다

    - Publisher가 emit한 데이터를 전달 받아 처리
    - Error가 발생할 경우 해당 에러를 전달 받아 처리
    - Reactor Sequence가 정상적으로 종료된 후 후처리
<br>

> 참고) [Project Reactor](https://spring.io/reactive)
