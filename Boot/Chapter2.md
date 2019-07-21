# 02 스프링 부트 리액티브 웹 

> ## 목차 

1. 스프링 이니셜라이저를 이용한 리액티브 웹 애플리케이션 생성하기 
2. 리액티브 프로그래밍의 원리 배우기 
3. 리액터 타입 소개하기
4. 아파치 톰캣에서 임베디드 네티로 전환하기 
5. 리액티브 **스프링 웹 플럭스** 와 고전적인 스프링 MVC 비교하기
6. 일부 모노/플럭스-기반 엔드포인트 표시하기
7. 리액티브 ImageService 생성하기 
8. 리액티브 파일 컨트롤러 생성하기 
9. 타임리프 템플릿과 상호 작용하는 방법 표시하기 
10. 비동기에서 동기화로 이동하는 방법을 설명하는 것은 쉽지만, 그 반대는 아니다. 

> ### 1. 스프링 이니셜라이저를 이용한 리액티브 웹 어플리캐이션 생성



> ### 2. 리액티브 프로그래밍의 원리 배우기 

#### 스프링 프레임워크 5 **리액티브**

리액티브 애플리케이션은 난블로킹, 비동기 오퍼레이션의 개념이다. 비동기란, 폴링이나 백엔드 이벤트에 의해 답변이 나중에 제공되는 것을 의미한다. 이를 이용하면 결과가 나오는 동안, **스레드를 유지하지 않고 다른 서비스를 호출할 수 있다**

- Push to Pull 

  시스템에서 이미지를 요청하는 경우를 생각해보자. 푸시 기반의 경우 메모리가 부족해질 위험을 대비해서 페이지-기반 솔루션을 사용한다. 

  ```java 
  # 수십만 개의 Image가 리턴될 수 있는 위험성
  public interface MyRepository {
    List<Image> findAll();
  }
  
  # 다음과 같이 변경 
  public interface MyRepository {
  	Page<Image> findAll(Pageable p);
  }
  ```

  리액티브 스트림을 사용하면 클라이언트가 가져올 항목 수를 선택할 수 있게 해주는 컨테이너를 리턴한다. 이는 1개이든, 수천개이든 클라이언트는 똑같은 메커니즘을 사용할 수 있다. 

  ```java
  public interface MyRepository {
    Flux<Image> findAll();
  }
  ```

  

> ### 3. 리액터 타입 소개 

리액티브 스트림을 사용한다고 말은 했지만 사실 초기 버전이기 때문에 애플리케이션을 빌드하는 데는 효과적이지 않다고 한다. 따라서 **프로젝트 리액터** 를 사용하여 애플리케이션을 빌드한다. 

>  :smile: 프로젝트 리액터(http://projectreactor.io/) 
>
> 스프링 프레임워크 5에서 리액티브 프로그래밍 모델에 사용하는 핵심 라이브러리 

> 리액터, Flux, Mono 요약 설명 : https://javacan.tistory.com/entry/Reactor-Start-1-RS-Flux-Mono-Subscriber


#### 플럭스 코드 살펴보기

```java
Flux.just("alpha", "beta", "gamma");
```

- **Flux** 는 리액터의 기본 타입으로 0..N의 아이템을 저장하는 컨테이너이다.

- `just()`

  : 고정 컬럭션을 구성하는 정적 헬퍼 메서드. `fromArray()` `fromIterable()` 및 `fromStream()` 과 같은 다른 정적 헬퍼도 사용 가능 

**Flux** 는 무엇일까? 

플럭스는 비동기적으로 제공되는 여러 값을 의미한다. 이러한 값들은 제공되었을 때 스레드에 도착한다고 보장할 수 없다. 플럭스를 소비하려면 구독(Subscribe)를 사용하거나 프레임워크를 통해야한다. 

```java
From.just("alpha", "beta", "gamma").subscribe(System.out::println);

/* console print
alpha
beta
gamma
*/
```

`Arrays.asList("alpha", "beta", "gamma")` 와 같은 기존 자바 컬렉션 빌더와 차이점은 무엇일까? 플럭스를 사용하면 일련의 함수 호출을 연결할 수 있다. 모든 함수 호출은 정확한 요소가 추출될 때까지 지연된다. 

```java
Flux.just(
  (Supplier<String>)()->"alpha",
  (Supplier<String>)()->"beta",
  (Supplier<String>)()->"gamma")
  .subscribe(supplier->System.out.println(supplier.get()));
)

Flux.just() or Flux.generate()는 데이터 생성과정 자체가 동기 방식이다. Subscriber로부터 데이터 요청이 오면 그 시점에 SynchronousSink를 이용해 데이터를 생성한다. 반면에 별도 쓰레드를 이용해서 비동기로 데이터를 생성해야 하는 경우에는 SynchronousSink를 사용할 수 없다. 또한, SynchoronousSink는 한 번에 하나의 next 신호만 발생할 수 있다. 

-> 이러한 경우에는 Flux.create()를 사용해야 한다.
```



> ### 5. 리액티브 스프링 웹 플럭스 VS. 스프링 MVC

리액티브 웹 플럭스를 사용하는 가장 큰 이점은 동일한 패러다임과 함께 @MVC와 동일한 어노테이션을 사용함과 동시에 입출력에서 리액터 타입(모노 및 플럭스)를 지원한다는 것이다. 



> ### 7. 모노/플럭스-기반 엔드포인트 표시 

스프링 웹 플럭스는 스프링 MVC 엔드포인트와 마찬가지로 플럭스 오퍼레이션을 지원한다. 

```java
@GetMapping(API_BASE_PATH + "/images")
Flux<Image> images() {
  return Flux.just(
    new Image("1", "learning-spring-boot-cover.jpg"),
    new Image("2", "learning-spring-2nd-edition-cover.jpg"),
    new Image("3", "dfdd.jpg")
  );
}
```

스프링 컨트롤러는 `Flux<Image> ` 리액터 타입을 리턴하고, 적절한 시기에 이 플로를 구독하는 책임을 스프링에게 맡김. 

