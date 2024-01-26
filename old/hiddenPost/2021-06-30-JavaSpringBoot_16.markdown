---
layout: post
title: "Java SpringBoot(16) - Spring 통합 테스트"
date: 2021-06-30 11:00:00 +0100
categories:
---

# Java SpringBoot(16) - Spring 통합 테스트

&nbsp;
&nbsp;
• **1. 통합 테스트 코드**
&nbsp;
**스프링 컨테이너와 DB까지 연결한 통합 테스트 진행**

```java

  package com.miguel.hellospring.service;

  import com.miguel.hellospring.domain.Member;
  import com.miguel.hellospring.repository.MemberRepository;
  import com.miguel.hellospring.repository.MemoryMemberRepository;
  import org.junit.jupiter.api.AfterEach;
  import org.junit.jupiter.api.BeforeEach;
  import org.junit.jupiter.api.Test;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.boot.test.context.SpringBootTest;
  import org.springframework.transaction.annotation.Transactional;

  import static org.assertj.core.api.Assertions.assertThat;
  import static org.junit.jupiter.api.Assertions.assertThrows;

  @SpringBootTest // 스프링 컨테이너와 테스트를 함께 실행한다
  @Transactional // Test 시작시 Transaction 실행 -> DB 쿼리 수행 -> 테스트 완료시 commit하지 않고 rollback. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
  class MemberServiceIntegrationTest {

      @Autowired MemberService memberService;
      @Autowired MemberRepository memberRepository;

      @Test
      void join() {
          //given ~가 주어져서
          Member member = new Member();
          member.setName("hello");

          //when ~했을 때
          Long saveId = memberService.join(member);

          //then ~가 나와야 해
          Member findMember = memberService.findOne(saveId).get();
          assertThat(member.getName()).isEqualTo(findMember.getName());
      }

      @Test
      public void exceptionDuplicatedUser() {
          //given
          Member member1 = new Member();
          member1.setName("spring");

          Member member2 = new Member();
          member2.setName("spring");

          //when
          memberService.join(member1);
          // member2를 회원가입 시켰을 때, 이름이 동일한 멤버 1이 있으므로 IllegalStateException이 터질 것임.
          IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

          //then
          assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원이다");
      }
  }

```

&nbsp;
&nbsp;

- @SpringBootTest // 스프링 컨테이너와 테스트를 함께 실행한다
- @Transactional // Test 시작시 Transaction 실행 -> DB 쿼리 수행 -> 테스트 완료시 commit하지 않고 rollback. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
