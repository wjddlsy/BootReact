# 1장 

## 시작하기 

### 1. 스프링 이니셜라이저 (https://start.spring.io)

스프링부트를 간단하게 시작할 수 있도록 도와주는 도구 



### 2. 스프링부트 프로젝트 구성 

- 프로젝트 폴더 구성 

```
learning-spring-boot
|__ build.gradle
|__ gradle 
		|__ wrapper
|__ src 
		|__ main
		    |__ java
		    |__ resources
		    		|__ application.properties
```

- build.gradle

  프로젝트의 기본을 나타낸다. 

```json
buildplugins {
	id 'org.springframework.boot' version '2.2.0.M4'
	id 'java'
}

apply plugin: 'io.spring.dependency-management'

group = 'com.greglturnquist.learningspringboot'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
	maven { url 'https://repo.spring.io/snapshot' }
	maven { url 'https://repo.spring.io/milestone' }
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-mongodb-reactive'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-webflux'
	compileOnly 'org.projectlombok:lombok'
  compileOnly 'de.flapdoodle.embed:de.flapdoodle.empbed.mongo'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
		exclude group: 'junit', module: 'junit'
	}
	testImplementation 'io.projectreactor:reactor-test'
}

test {
	useJUnitPlatform()
}

```



