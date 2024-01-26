---
layout: post
title: "Java SpringBoot(19) - Spring Data JPA"
date: 2021-07-02 11:00:00 +0100
categories:
---

# Java SpringBoot(19) - Spring Data JPA

&nbsp;
&nbsp;
• **1. Spring Data JPA?**
&nbsp;

- Repository에 구현 클래스 없이 인터페이스만으로 개발을 완료할 수 있도록 도와준다.
- 반복적으로 개발해왔던 기본적인 CRUD 기능을 인터페이스를 통해 제공한다.
- findByName() , findByEmail() 처럼 메서드 이름 만으로 조회 기능 제공
- 페이징 기능 자동 제공
- 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 된다. Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, 앞서 학습한 스프링 JdbcTemplate를 사용하면 된다.

&nbsp;
&nbsp;
• **2. 스프링 데이터 JPA 회원 Repository**
&nbsp;

```java
  // /repository/SpringDataJpaMemberRepository (클래스가 아닌 interface임.)
  package com.miguel.hellospring.repository;

  import com.miguel.hellospring.domain.Member;
  import org.springframework.data.jpa.repository.JpaRepository;

  import java.util.Optional;

  public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository{
      // JpaRepository를 extend 하고 있으면, spring data jpa가 구현체를 자동으로 만들어 spring bean에 자동으로 등록한다.

      // select m from Member m where m.name = ? 이라는 JPQL
      @Override
      Optional<Member> findByName(String name);
  }
```

&nbsp;
&nbsp;
• **3. 스프링 데이터 JPA 회원 Repository를 사용하도록 스프링 설정 변경**
&nbsp;

```java
  // /SpringConfig
  .
  .
  .
  @Configuration
  public class SpringConfig {

      private final MemberRepository memberRepository;

      public SpringConfig(MemberRepository memberRepository) {
          this.memberRepository = memberRepository;
      }

      @Bean // 이 부분의 logic을 읽고 Spring Bean 에 등록
      public MemberService memberService() {
          // @Bean 을 모두 spring bean에 등록하고, 등록된 memberRepository를 MemberService에 넣어준다.
          return new MemberService(memberRepository);
      }
  }
```

&nbsp;

- 스프링 데이터 JPA가 SpringDataJpaMemberRepository 를 스프링 빈으로 자동 등록해준다.

&nbsp;
&nbsp;
• **4. Spring Data JPA 제공 클래스**
&nbsp;
![스프링 데이터 JPA 제공 클래스](../../../../assets/images/SpringDataClass.png)
&nbsp;
&nbsp;
