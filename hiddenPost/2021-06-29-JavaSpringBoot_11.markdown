---
layout: post
title:  "Java SpringBoot(11) - Spring Bean ~ Java code로 직접 등록"
date:   2021-06-29 11:00:00 +0100
categories:
---

# Java SpringBoot(11) - Spring Bean ~ Java code로 직접 등록
&nbsp;
&nbsp;
• **1. SpringConfig 코드**
&nbsp;
```

  package com.miguel.hellospring;

  import com.miguel.hellospring.repository.MemberRepository;
  import com.miguel.hellospring.repository.MemoryMemberRepository;
  import com.miguel.hellospring.service.MemberService;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;

  // Spring이 뜰 때 이 Configuration을 읽고 이를 spring Bean에 등록한다.
  @Configuration
  public class SpringConfig {

      @Bean // 이 부분의 logic을 읽고 Spring Bean 에 등록
      public MemberService memberService() {
          // @Bean 을 모두 spring bean에 등록하고, 등록된 memberRepository를 MemberService에 넣어준다.
          return new MemberService(memberRepository());
      }

      @Bean
      public MemberRepository memberRepository() {
          // 시나리오상 아직 DB가 확정되지 않은 상태임.
          // 향후 DB가 확정되어 구성이 완료되었다면, 이 부분만 바꾸어주면 된다.
          return new MemoryMemberRepository();
      }
      
  }

```
&nbsp;
&nbsp;
• **2. DI 방식의 장단점**
&nbsp;
- Dependency Injection에는 필드 주입, 세터 주입, 생성자 주입의 세 가지 방법이 있다.
- 생성자 주입 - Argument에 생성자를 통해 만들어 낸 컴포넌트를 주입하는 방식
- 필드 주입 - 생성자 없이, 필드 자체에 @Autowired를 주어 주입하는 방식. 추천하는 방식이 아님.
- 세터 주입 - 생성은 생성자가 따로 해주고, 세터에 @Autowired를 주어 주입하는 방식. 한 번 주입된 후에도 세터는 public으로 노출되어 있기 때문에 중도에 잘못된 값으로 변경될 수 있다.(조립 시점에 한 번만 호출되어 조립한 후 다시는 호출되면 안된다)
- 의존 관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입이 권장된다.
- 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용하고, 정형화 되지 않거나 상황에 따라 구현 클래스를 변경해야 하는 경우에 설정을 통해 스프링 빈으로 등록한다.
- Autowired를 통한 DI는 Controller, Service 등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.

&nbsp;
