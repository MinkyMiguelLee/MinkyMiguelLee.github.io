---
layout: post
title: "Java SpringBoot(18) - JPA"
date: 2021-07-01 11:00:00 +0100
categories:
---

# Java SpringBoot(18) - JPA

&nbsp;
&nbsp;
• **1. JPA?**
&nbsp;

- Java Persistence API
- JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다.
- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.
- JPA를 사용하면 개발 생산성을 크게 높일 수 있다.

&nbsp;
&nbsp;
• **2. JPA 사용을 위한 설정**
&nbsp;
• **build.gradle 파일에 JPA, H2 관련 라이브러리 추가**

```java

  dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    //implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa' // 이 부분 추가
    runtimeOnly 'com.h2database:h2'
    testImplementation('org.springframework.boot:spring-boot-starter-test') {
      exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }
  }

```

&nbsp;

- spring-boot-starter-data-jpa 는 내부에 jdbc 관련 라이브러리를 포함한다. 따라서 jdbc는 제거해도 된다.

&nbsp;
&nbsp;
• **스프링 부트에 JPA 설정 추가**

```java

  // /resources/application.properties
  spring.datasource.url=jdbc:h2:tcp://localhost/~/test
  spring.datasource.driver-class-name=org.h2.Driver
  spring.datasource.username=sa

  spring.jpa.show-sql=true
  spring.jpa.hibernate.ddl-auto=none

```

&nbsp;

- show-sql : JPA가 생성하는 SQL을 출력한다.
- ddl-auto : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none 를 사용하면 해당 기능을 끈다.
- create 를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다.

&nbsp;
&nbsp;
• **JPA Entity Mapping**

```java

  // /domain/Member

  package com.miguel.hellospring.domain;

  import javax.persistence.Entity;
  import javax.persistence.GeneratedValue;
  import javax.persistence.GenerationType;
  import javax.persistence.Id;

  @Entity // 테이블 매핑
  public class Member {

      @Id // PK 매핑
      @GeneratedValue(strategy = GenerationType.IDENTITY) // DB가 알아서 생성해주는 PK의 경우
      private Long id;
      private String name;

      public Long getId() {
          return id;
      }

      public void setId(Long id) {
          this.id = id;
      }

      public String getName() {
          return name;
      }

      public void setName(String name) {
          this.name = name;
      }
  }

```

&nbsp;
&nbsp;
• **3. JPA 회원 Repository**

```java

  // /repository/JpaMemberRepository

  package com.miguel.hellospring.repository;

  import com.miguel.hellospring.domain.Member;

  import javax.persistence.EntityManager;
  import java.util.List;
  import java.util.Optional;

  public class JpaMemberRepository implements MemberRepository{

      private final EntityManager em; // JPA 사용을 위해 만들어줘야 함

      public JpaMemberRepository(EntityManager em) {
          this.em = em;
      }

      @Override
      public Member save(Member member) {
          em.persist(member); // 영속성... 저장을 위해 사용. ID도 자동으로 만들어 준다.
          return member;
      }

      @Override
      public Optional<Member> findById(Long id) {
          Member member = em.find(Member.class, id);
          return Optional.ofNullable(member);
      }

      @Override
      public Optional<Member> findByName(String name) {
          // 객체에 쿼리하는 방식. 각 컬럼을 따로 받아 객체로 묶어주거나 하는 작업 없이 바로 객체 형태로 result를 받아볼 수 있다.
          List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                  .setParameter("name", name)
                  .getResultList();

          return result.stream().findAny();

      }

      @Override
      public List<Member> findAll() {
          List<Member> result = em.createQuery("select m from Member m", Member.class)
                  .getResultList();
          return result;
      }
  }

```

&nbsp;
&nbsp;
• **4. 서비스 계층에 Transaction 추가**

```java

  // /service/MemberService

  .
  .
  .
  import org.springframework.transaction.annotation.Transactional

  @Transactional
  public class MemberService {}
  .
  .
  .

```

&nbsp;

- 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋한다. 만약 런타임 예외가 발생하면 롤백한다.
- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.

&nbsp;
&nbsp;
• **5. JPA를 사용하도록 스프링 설정 변경**

```java

  // SpringConfig

  .
  .
  .

  import javax.persistence.EntityManager;

  @Configuration
  public class SpringConfig {

      private EntityManager em;

      @Autowired
      public SpringConfig(EntityManager em) {
          this.em = em;
      }

      .
      .
      .

      @Bean
      public MemberRepository memberRepository() {
          return new JpaMemberRepository(em);
      }
  }

```
