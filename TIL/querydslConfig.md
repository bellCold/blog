## Querydsl
Querydsl이란 Java 언어로 작성된 타입 세이프한 SQL 쿼리를 생성하기 위한 자바 라이브러리입니다.
Querydsl의 주요 기능에는 타입 세이프한 쿼리 작성, 동적 쿼리 작성, 조건문 및 연산자 지원, 코드 기반 쿼리 작성, 페이징 및 정렬 지원 등이 있습니다.

## Querydsl 설정하기 -> build.gradle
querydsl을 시작하려면 몇 가지 설정을 project에 해줘야하는데 최근 설정을 진행하였을때 최신 version에 따른 몇 가지 이슈를 겪은것을 정리

우선 관련 라이브러리들을 추가해줍니다.

```java
implementation "com.querydsl:querydsl-jpa:5.0.0:jakarta"
    annotationProcessor "com.querydsl:querydsl-apt:5.0.0:jakarta"
    annotationProcessor "jakarta.annotation:jakarta.annotation-api"
    annotationProcessor "jakarta.persistence:jakarta.persistence-api"
```

java -> jakarta로 바뀌면서 아래 두가지 자카르타 어노테이션관련된 것들도 추가해줘야합니다.

```java
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.0.6'
    id 'io.spring.dependency-management' version '1.1.0'
    // querydsl 설정
    id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
}
```

```
def generated = 'src/main/generated'

querydsl {
    jpa = true
    querydslSourcesDir = generated
}
sourceSets {
    main.java.srcDir generated
}

compileQuerydsl{
    options.annotationProcessorPath = configurations.querydsl
}

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
    querydsl.extendsFrom compileClasspath
}
```

나머지 Application Config를 설정해줍니다.

```java
@Configuration
public class QuerydslConfig {

    @Bean
    public JPAQueryFactory jpaQueryFactory(EntityManager entityManager) {
        return new JPAQueryFactory(entityManager);
    }
}
```

이렇게 설정하면 querydsl을 사용할 준비가 끝났습니다.