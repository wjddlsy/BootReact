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
</pre>



> ### 4. 스프링 부트의 속성 지원에 대해 알아보기 

- 스프링 부트는 미리 빌드된 속성을 가짐 

- 자동 설정 컴포넌트는 속성 설정이 있어서 원하는 부분만 오버라이드 가능 

- 부트는 사용자가 직접 정의한 빈을 발견하면 자동 환경 설정 빈을 삭제하고 사용자 정의 빈을 사용

  -> 이로 인해 다른 컴포넌트가 중지될 수 있는 가능성이 존재함 

- 따라서, 일부 속성만 조정하고 싶다면 빈 전체를 바꿀 필요 없이 자동 설정 빈의 속성을 조정할 수 있다. 

```
# src/main/resources/application.properties 

# 톰캣 포트 
server.port=9000

# 커스텀 로그 레벨
logging.level.com.greglturnquist=DEBUG
```

위와 같이 수정하면 스프링 부트는 9000포트에서 네티를 시작하고 로그 레벨을 DEBUG로 설정할 수 있다..

#### 스프링부트가 오버라이드를 지원하는 이유 

앱의 JAR 파일 내에만 구성 설정을 포함시켜야 한다면 구성 설정이 바뀔 때마다 다시 빌드해야하고 불편하기 때문에 속성 파일을 ==외부화하는 것이 편리==

#### 스프링 부트가 속성 오버라이드를 지원하는 항목

- 테스트 클래스의 @TestPropertySource 어노테이션
- 커맨드라인 파라미터
- SPRING_APPLICATION_JSON 내의 속성 
- ServletConfig 초기화 파라미터 
- ServletContext 초기화 파라미터
- java:comp/env의 JNDI 속성
- 자바 시스템 속성 (System.getProperties())
- OS 환경 변수 
- RandomValuePropertySource는 random.*에 있는 속성만을 가진다.
- 패키지된 JAR 파일 외부의 프로파일별 속성 (application-{profile}.properties 및 YAML 문서)
- 패키지된 JAR 파일 내부의 프로파일별 속성(application-{profile}.properties 및 YAML 문서)
- 패키지된 JAR 파일 외부의 어플리케이션 속성(application.properties 및 YAML 문서)
- 패키지된 JAR 파일 내부의 어플리케이션 속성(application.properties 및 YAML 문서)
- @Configuration 클래스의 @PropertySource 어노테이션
- 기본 속성(SpringApplication.setDefaultProperties를 사용해 지정됨)

application.properties 을 YAML 형식으로 대체하고 싶다면 다음과 같이 작성할 수 있다 

```
server:
	port:9000
loggin:
	level:
	  com:
	  	greglturnquist: DEBUG
```



#### properties VS. YAML

> properties로 사용하는 모든 설정은 yaml으로 대체 가능

YAML의 장점 

1. 계층구조 표현에 더 적합하다. 
2. key, value 매핑을 간단하게 명시적으로 가능하다. 
3. 문서 중심의 구조화가 가능하다. 



스프링 속성은 다른 속성을 참조할 수 있다. 

```
app.name=Myapp
app.description=${app.name} is a Spring Boot application
```



> ### 5. 애플리케이션을 실행 가능한 JAR 파일로 묶기 

`spring-boot-gradle-plugin`은 프로덕션을 처리할 수 있는 후크를 내장하고 있다. 

:smile: **web-hook** : 서버에서 어떤 작업이 수행되었을 때 해당 작업이 수행되었음을 HTTP POST로 알리는 개념 

#### JAR 만들기 

그래들의 빌드 작업을 호출하여 빌드 프로세스에 자신을 삽입하고 JAR 파일을 만든다

```bash
$ ./gradlew clean build
```

build 디렉토리가 생성되며 build/libs 디렉토리에 jar 파일이 생성된다. 

#### JAR 실행하기 

```bash
$ java -jar build/libs/learning-spring-boot-0.0.1-SNAPSHOT.jar
```

자바의 `-jar` 옵션을 이용해 JAR를 호출하면 시스템의 JVM만으로 애플리케이션을 시작할 수 있다. 

설정을 오버라이드할 경우, 설정 파일을 열어서 수정할 수 도 있고 실행할 때 지정할 수도 있다. 

```bash
$ SERVER_PORT=8000 java -jar build/libs/learning-spring-boot-0.0.1-SNAPSHOT.jar
```



> ### 6. 클라우드 파운드리에 배포

클라우드-네이티브는 애플리케이션 표준으로 자리잡고 있다 

:smile: **클라우드-네이티브**

​	규모에 맞게 소프트웨어를 빠르고, 일관되며, 안정적으로 제공하는 고성능 조직 패턴. 지속적인 배포, 데브옵스 및 마이크로서비스는 클라우드-네이티브의 이유, 방법 및 종류를 나타낸다. 



> ### 7. 프로덕션-준비 지원 추가

프로덕션에 필요한 기능들을 생각해보자 

1. 애플리케이션의 작동 여부를 확인하기 위한 모니터링 소프트웨어 구성 
2. 앱을 사용하는 사람들의 매트릭스(통계 지표)
3. 오류의 원인 찾기 

#### 스프링 부트 액추에이터 

```
# build.gradle
compile('org.springframework.boot:spring-boot-starter-actuator')
```

| 액추에이터 엔드포인트    | 설명                                                         |
| ------------------------ | ------------------------------------------------------------ |
| /application/autoconfig  | 스프링부트가 자동 설정한 것, 하지 않은 것 그리고 이유를 리포트 |
| /application/beans       | 애플리케이션 컨텍스트에 설정된 모든 빈을 리포트              |
| /application/configprops | 모든 설정 속성을 표시                                        |
| /application/dump        | 스레드 덤프 리포트가 생성됨                                  |
| /application/env         | 현재 시스템 환경을 리포트                                    |
| /application/health      | 앱의 헬스를 확인하는 간단한 엔드포인트                       |
| /application/info        | 앱에서 커스텀 컨텐츠 제공                                    |
| /application/metrics     | 웹 사용에 대한 카운터 및 게이지를 보여준다                   |
| /application/mappings    | 모든 스프링 웹 플럭스 라우트에 대한 세부 정보를 얻을 수 있다. |
| /application/trace       | 과거 요청에 대한 세부 정보를 보여줌                          |

엔드포인트는 기본적으로 비활성화되어있고 아래와 같이 활성화할 수 있다. 

```
# src/main/resources/application.properties
endpoints.{endpoint}.enabled = true 
```



