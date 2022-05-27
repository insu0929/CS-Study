# 1. WebFlux 개요

# Spring WebFlux

Spring 프레임워크에서 reactive 어플리케이션을 개발할 수 있게 도와주는 모듈

**필수의존성**

- spring-boot-starter-webflux
- spring-boot-starter-web와 함께 추가 시 MVC로 웹 애플리케이션이 적용됨

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

# Reactive Stream

- Reactive 프로그래밍을 위한 기본 인터페이스 명세
- 논블로킹(Non-blocking) 백 프레셔(back pressure)를 이용한 비동기 데이터 처리의 표준
- publisher, subscription, subscriber, processor로 구성

### [publisher](https://www.reactive-streams.org/reactive-streams-1.0.3-javadoc/org/reactivestreams/Publisher.html?is-external=true)

```java
public interface Publisher<T> {
    void subscribe(Subscriber<? super T> s);
}
```

- 데이터 제공자, 자신을 구독한 Subscriber 가 Subscription 객체를 통해 데이터 요청시, 요청된 양만큼 Subscriber에게 데이터 전송
- Subscriber가 Publisher 의 subscribe 메서드 호출하여 구독시, Publisher 는 subscribe 메서드에서 해당 Subscriber의 OnSubscribe 메서드 호출하여 자신의 Subscription 제공

### [subscriber](https://www.reactive-streams.org/reactive-streams-1.0.3-javadoc/org/reactivestreams/Subscriber.html)

```java
public interface Subscriber<T> {
    void onSubscribe(Subscription s);
    void onNext(T t);
    void onError(Throwable t);
    void onComplete();
}
```

- Publisher 에게 데이터를 요청하는 요청자
- Publisher.subscribe가 호출될 때 onSubscribe가 호출된다
- onNext를 통해 Publisher가 전송해주는 데이터 수신, onError/onComplete를 통해 데이터 수신 완료 처리


### [subscription](https://www.reactive-streams.org/reactive-streams-1.0.3-javadoc/org/reactivestreams/Subscription.html)

```java
public interface Subscription {
    void request(long n);
    void cancel();
}
```

- Subscriber와 Publisher 간 통신의 매개체
    - Publisher 가 자신의 Subscription 구현체를 Subscriber 에게 제공
- request(n) 을 통해 Subscriber 가 Publisher 에게 n개의 데이터 요청(backPressure)
- cancel() 을 통해 Subscriber 가 Publisher 에 대한 구독 취소
- 하나의 subscription은 하나의 subscriber와 대응된다

### Reactive Streams 흐름

![Untitled](https://engineering.linecorp.com/ko/reactivestreams1-10/)

1. Subscriber가 subscribe 함수를 사용해 Publisher에게 구독을 요청
2. Publisher는 onSubscribe 함수를 사용해 Subscriber에게 Subscription을 전달
3. Subscriber는 Subscription::request를 통해 Publisher 에게 데이터 요청
4. Publisher는 Subscription을 통해 Subscriber의 onNext에 데이터를 전달(요청 수만큼)
5. Publisher는 작업이 완료되면 onComplete, 에러가 발생하면 onError 시그널을 전달. Subscriber 는 onError/onComplete에서 그에 맞는 처리 수행