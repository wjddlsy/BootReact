> ## 목차
1. http://start.spring.io의 스프링 이니셜라이저를 사용해 베어 프로젝트 생성
2. 스프링 부트의 서드파티 라이브러리 관리에 대해 알아보기
3. 스탠드얼론 컨테이너가 없는 통합 개발 환경(IDE)에서 앱을 구동하는 방법 살펴보기
4. 스프링 부트의 속성 지원을 사용해 외부 조정하기
5. 자체 포함된 실행 가능한 JAR 파일의 앱 패키징하기
6. 클라우드에 앱 배포하기
7. 즉시 사용할 수 있는 프로덕션 레벨의 지원 도구 추가하기

> 스프링 부트 2.0 요약 : https://brunch.co.kr/@springboot/55
<hr/>

> ### 0. 다운로드

1. JDK 8
2. MongoDB 3.0
3. RabbitMQ 3.6
4. IDE (IntelliJ or STS)

<hr/>

> ### 1. 시작하기
<pre>
스프링 이니셜라이저에서 앱에 대한 최소한의 세부 사항입력 후 프로젝트 생성

[프로젝트 생성시 선택 사항]
1) Maven or Gradle project
2) Spring Boot version
3) Group
4) Artifact
5) Dependencies

[기본 Dependencies]
1) Reactive Web
2) Reactive MongoDB
3) Thymeleaf
4) Lombok(POJO)

[생성된 프로젝트 구성요소]
1) build.gradle
2) gradle
3) LearningSpringBootApplication.java
4) application.properties
5) LearningSpringBootApplicationTest.java
</pre> 

<hr/>

> ### 2. 스프링 부트 스타터
<pre>
1. 의존성을 지정하지 않으면 애플리케이션이 완료되지 않는다. 스프링 부트의 중요한 기능은 가상 패키지이다.
2. 스프링 부트 스타터 패키지를 사용하면 설치 및 실행에 필요한 부분을 신속하게 파악할 수 있다.

[주요 스타터 라이브러리]
1. Reactive MongoDB
2. Thymeleaf
3. Webflux

[추가 라이브러리]
1. Lombok : 접근자, 설정자 및 기타 세부사항 설정없이 POJO 정의
2. Flapdoodle : 외부 데이터베이스를 사용하기 전에 테스트를 작성하고, 솔루션을 검새하고, 작업을 수행할 수 있도록 해주는 임베디드 몽고 DB
3. spring-boot-starter-test: 테스트 범위내의 모든 기능 포함
</pre>

> ### 3. 스프링 부트 애플리케이션 실행
<pre>
LearningSpringBootApplication.java

# @SpringBootApplication : 스프링 부트가 실행될 때 패키지 내의 스프링 컴포넌트를 재귀적으로 스캔해 등록하도록 지시한다. 또한 스프링 부트가 클래스패스 설정, 속성 설정 및 기타 요소에 따라 빈이 자동으로 생성되도록 하는 '자동 설정'을 실행하도록 지시한다.

# SpringApplication.run() 
1) 스프링 부트 버전 확인
2) 임베디드 네티 포트 확인
3) 임베디드 몽고 DB 저장소인 플랩두들 확인 

# 임베디드 네티 : 
리액티브 애플리케이션은 비동기, Non-Blocking 하게 동작한다. 스프링 부트 2.0 에서는 리액티브 애플리케이션 개발을 위한, 오토 컨피그레이션을 제공한다. 기존 톰캣 임베디드 방식에더해 2.0부터는 리액티브 환경을 위한 Netty 등의 임베디드 서버 구성을 지원한다.

# 스프링 웹 플럭스(Spring WebFlux) :
설명 : https://brunch.co.kr/@springboot/96

# ReactiveCrudRepository :
1) 도메인 정보를 캡처하는 동시에 CI(concrete implementation)을 만든다. 또한 미리 정의된 CRUD 오퍼레이션(save, delete, deleteBy, deleteAll, findById, findAll)도 제공한다.
2) ReactiveCrudRepository<Chapter, String> 으로 엔티티 타입과, 기본 키 타입을 지정한다.
3) 커스텀 파인더 : !! 3장에서 진행

</pre>

