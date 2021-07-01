---
layout: post
title:  "Java SpringBoot(9) - Service 테스트케이스 작성 / dependency injection"
date:   2021-06-25 10:00:00 +0100
categories:
---

# Java SpringBoot(9) - Service 테스트케이스 작성 / dependency injection
&nbsp;
&nbsp;
• **1. MemberService의 TestCase 작성**
&nbsp;
- 해당 클래스에서 Command + shift + t : 해당 클래스에 대한 새로운 테스트코드 생성
- get~when~then 방식 : get, when, then을 순서대로 주석으로 달아놓고, 테스트코드를 작성하면 훨씬 이해하기 쉽고 간단하게 작성이 가능하다. ~가 주어져서, ~했을 때, ~가 나와야 한다...

&nbsp;
&nbsp;
• **2. MemberServiceTest 코드**
```
-----------------------------------------------------------------------
  package com.miguel.hellospring.service;

  import com.miguel.hellospring.domain.Member;
  import com.miguel.hellospring.repository.MemoryMemberRepository;
  import org.assertj.core.api.Assertions;
  import org.junit.jupiter.api.AfterEach;
  import org.junit.jupiter.api.BeforeEach;
  import org.junit.jupiter.api.Test;

  import static org.assertj.core.api.Assertions.assertThat;
  import static org.junit.jupiter.api.Assertions.*;

  class MemberServiceTest {

      MemberService memberService;
      MemoryMemberRepository memberRepository;
      // MemberService 에 있는 repository와 테스트에서의 Repository는 각각 new를 통해 만들어진 별개의 객체임

      @BeforeEach // 매 메서드 실행 전 수행
      public void beforeEach() {
          memberRepository = new MemoryMemberRepository();
          memberService = new MemberService(memberRepository);
          // 하나의 Repository를 공유하기 위해 생성한 memberRepository를 memberService에 전달
      }

      @AfterEach
      public void afterEach() {
          memberRepository.clearStore();
      }

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
          assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다");
      }

      @Test
      void findMembers() {
      }

      @Test
      void findOne() {
      }
  }
-----------------------------------------------------------------------
```
&nbsp;
&nbsp;
• **3. dependency injection**
&nbsp;
- 위의 테스트케이스 코드에서, afterEach의 clearStore() 작업을 수행하기 위해 MemoryMemberRepository 객체를 생성했다.
- 그런데, 이 경우 MemberService가 사용하고 있는 Repository와는 별개의 또 다른 객체가 생성되는 것이다.
- 따라서, 한 Repository를 공유하기 위해서 DI 작업을 수행해주어야 한다.
- DI란 

> *DI(Dependency Injection)는*
> *각 객체간의 의존성을 스프링 컨테이너(Spring Container)가 자동으로 연결해주는 것으로*
> *개발자가 빈(Bean) 설정파일에 의존관계가 필요한 정보를 추가해주면 스프링 컨테이너가 자동적으로 연결해줍니다.* 
> *쉽게 말하자면, 의존성 주입 말 그대로 의존성을 주입시켜준다는 의미입니다.*
> *개발자가 객체를 직접 생성하는 방식이 아니라 외부에서 생성하여 주입 시켜주는 방식입니다.* 
> *의존성 주입은 IoC(Inversion of Control, 제어의 역전) 원칙 하에 객체 간의 결합을 약하게 해주고* 
> *유지보수가 좋은 코드를 만들어 줍니다. 또한 의존성 주입은 개발자들이 객체를 생성하는 번거로움과* 
> *다양한 케이스를 고려하는 경우를 줄이고, 변수 사용과 개발에 더욱이 집중할 수 있게 해줍니다.*

&nbsp;
&nbsp;
• **4. 변경된 MemberService 코드**
&nbsp;
```
-----------------------------------------------------------------------
  package com.miguel.hellospring.service;

  import com.miguel.hellospring.domain.Member;
  import com.miguel.hellospring.repository.MemberRepository;
  import com.miguel.hellospring.repository.MemoryMemberRepository;

  import java.util.List;
  import java.util.Optional;

  public class MemberService {

      private final MemberRepository memberRepository;

      /*
          private final MemberRepository memberRepository = new MemoryMemberRepository();
          형식으로 새로 만들어줬던 repository를 아래의 constructor 방식으로 외부에서 Argument로 전달하도록
          변경함. 이러면 하나의 Repository를 공유할 수 있음
      */
      public MemberService(MemberRepository memberRepository) {
          this.memberRepository = memberRepository; //
      }
  .
  .
  .
  }
-----------------------------------------------------------------------
```