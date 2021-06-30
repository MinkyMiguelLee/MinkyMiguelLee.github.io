---
layout: post
title:  "Java SpringBoot(15) - JDBC 연동"
date:   2021-06-30 11:00:00 +0100
categories:
---

# Java SpringBoot(15) - JDBC 연동
&nbsp;
&nbsp;
• **1. JDBC 환경 설정**
&nbsp;
• **build.gradle 파일에 jdbc, h2 DB 관련 라이브러리 추가**
```
  plugins {
	id 'org.springframework.boot' version '2.5.1'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
  }

  group = 'com.miguel'
  version = '0.0.1-SNAPSHOT'
  sourceCompatibility = '11'

  repositories {
    mavenCentral()
  }

  dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    // 이 두 줄 추가함
    implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    runtimeOnly 'com.h2database:h2'
    // 이 두 줄 추가함
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
  }

  test {
    useJUnitPlatform()
  }

```
&nbsp;
&nbsp;
• **스프링 부트 데이터베이스 연결 설정 추가**
```
  // /resources/application.properties
  spring.datasource.url=jdbc:h2:tcp://localhost/~/test
  spring.datasource.driver-class-name=org.h2.Driver
  spring.datasource.username=sa
```
&nbsp;
&nbsp;
• **JDBC Repository 구현(생략)**
&nbsp;
&nbsp;
• **스프링 설정 변경**
```
  // /main/java/com.miguel.hellospring/SpringConfig
  
  .
  .
  .

  // Spring이 뜰 때 이 Configuration을 읽고 이를 spring Bean에 등록한다.
  @Configuration
  public class SpringConfig {

      private DataSource dataSource; // dataSource를 연결함

      @Autowired
      public SpringConfig(DataSource dataSource) {
          this.dataSource = dataSource;
      }

      @Bean // 이 부분의 logic을 읽고 Spring Bean 에 등록
      public MemberService memberService() {
          // @Bean 을 모두 spring bean에 등록하고, 등록된 memberRepository를 MemberService에 넣어준다.
          return new MemberService(memberRepository());
      }

      @Bean
      public MemberRepository memberRepository() {
          // return new MemoryMemberRepository();
          return new JdbcMemberRepository(dataSource);
          // 다른 코드를 손대지 않고, DataSource를 Autowired로 연결해 주고 memberRepository를 변경해 주기만
          // 했을 뿐인데, 정상적으로 H1 DB 사용이 가능하다.
      }
  }
```
&nbsp;
- DataSource는 데이터베이스 커넥션을 획득할 때 사용하는 객체다. 스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 DataSource를 생성하고 스프링 빈으로 만들어둔다. 그래서 DI를 받을 수 있다.
- 스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다.

&nbsp;
&nbsp;
![구현 클래스 추가 이미지](../../../../assets/images/AddClass.png)
&nbsp;
![스프링 설정 이미지](../../../../assets/images/SpringConfig.png)
&nbsp;