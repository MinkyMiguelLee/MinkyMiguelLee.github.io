---
layout: post
title:  "Java SpringBoot(10) - Spring Bean ~ component Scan"
date:   2021-06-25 10:00:00 +0100
categories:
---

# Java SpringBoot(10) - Spring Bean ~ component Scan
&nbsp;
&nbsp;
• **1. Controller 코드**
&nbsp;
```
  package com.miguel.hellospring.controller;

  import com.miguel.hellospring.service.MemberService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Controller;

  @Controller
  public class MemberController { // spring container가 뜰 때 생성한다.

      /*
          controller가 MemberService를 가져다 써야 한다.
          하지만, Spring이 관리를 하게 되면, 모두 Spring Container에 등록을 하고, 받아 쓰도록
          바꿔줘야 한다.
      */

      private  final MemberService memberService;

      @Autowired // 생성자 호출시 MemberService를 가져다 자동으로 연결시켜 준다.
      public MemberController(MemberService memberService) {

          /*
          그런데, 스프링 빈으로 등록되지 않은 서비스는 등록되지 않는다.
          why? Annotation이 되지 않은 클래스는 순수한 Java class 일 뿐이기 때문.
          따라서 해당 서비스에 @Service annotation을 적용해야 함.
          컨트롤러에는 @Controller, 서비스에는 @Service, 리포지토리에는 @Repository

          Autowired Annotation 역시, 각 생성자에 적용해야 한다.
          */

          this.memberService = memberService;
      }
  }
```
&nbsp;
&nbsp;
- 생성자에 @Autowired가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아 넣어준다. 이렇게 객체 의존 관계를 외부에서 넣어주는 것을 DI라고 한다.
- 이전 테스트에서는 개발자가 직접 주입했고(new로 새로운 객체 생성), 여기서는 @Autowired에 의해 스프링을 주입한다.
- controller가 MemberService를 가져다 써야 한다. 하지만, Spring이 관리를 하게 되면, 모두 Spring Container에 등록을 하고, 받아 쓰도록 바꿔줘야 한다.
- 스프링 빈으로 등록되지 않은 서비스는 @Autowired를 사용해도 연결할 수 없다. why? Annotation이 되지 않은 클래스는 순수한 Java class 일 뿐이기 때문.
- 따라서 해당 서비스 컨트롤러에는 @Controller, 서비스에는 @Service, 리포지토리에는 @Repository 적용해야 함.
- @Autowired Annotation 역시, 각 생성자에 적용해야 한다.

&nbsp;
&nbsp;
• **2. Component Scan**
&nbsp;
- 위와 같이 @Controller, @Service, @Repository같은 Annotation을 통해 의존관계를 주입하는 것을 Component Scan이라고 한다.
- 각각의 Controller, Service, Repository Annotation 내에는 @Component라는 Annotation이 들어있다. 즉, 특수한 확장 버전인 것
- 스프링이 올라올 때, 각각의 Component Annotation이 있는 Class가 있으면 해당 Class의 객체를 하나씩 만들어 Spring Container에 등록한다.
- 이렇게 Container에 등록된 컴포넌트들을 @Autowired를 통해 연결하여 사용하는 것.
- 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본적으로 싱글톤(유일하게 하나만 등록해서 공유한다)으로 등록한다.
- 이 외에, 자바 코드로 직접 스프링 빈을 등록하는 방법도 있다.

&nbsp;
- *@Component* Annotation 이 있으면 스프링 빈으로 자동 등록
- *@Controller* 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.
- *@Component* 를 포함하는 @Repository, @Service, @Controller도 자동 등록된다.
- 기본적으로, 메인 코드가 위치한 경로로부터 하위 경로들은 스프링이 모두 뒤져서 자동으로 Spring Bean으로 등록하지만,
- 하지만, 동일한 높이이거나 메인 코드보다 상위 경로는 스캔하지 않는다.
&nbsp;
&nbsp;
![SpringBoot Spring Bean - Component Scan](../../../../assets/images/springBean.png)
