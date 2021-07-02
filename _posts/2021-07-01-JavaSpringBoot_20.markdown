---
layout: post
title:  "Java SpringBoot(20) - AOP"
date:   2021-07-01 11:00:00 +0100
categories:
---

# Java SpringBoot(20) - AOP
&nbsp;
&nbsp;
• **1. AOP?**
&nbsp;
- Aspect-Oriented Programming.
- 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리

&nbsp;
![AOP Aspect 분리](../../../../assets/images/AopAspect.png)
&nbsp;
&nbsp;
• **2. AOP가 필요한 상황**
&nbsp;
- 모든 메소드의 호출 시간을 측정하고 싶다면?
- 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?
- 위와 같은 경우, AOP가 아닌 일반적 개발 방식에서는 같은 기능을 반복적으로 원하는 모든 메소드에 추가해 주어야 한다.
- 이럴 때, 공통의 AOP를 등록하여 사용할 수 있다.
- AOP를 사용하면, 수정이 필요할 때 해당하는 AOP의 로직만 수정하면 된다.
- 또한, 이 AOP를 적용할 대상을 원하는 대로 선택할 수 있다.

&nbsp;
&nbsp;
• **3. AOP 예시**
&nbsp;
```
  // /aop/TimeTraceAop
  package com.miguel.hellospring.aop;

  import org.aspectj.lang.ProceedingJoinPoint;
  import org.aspectj.lang.annotation.Around;
  import org.aspectj.lang.annotation.Aspect;

  @Aspect // AOP 적용을 위한 Annotation
  public class TimeTraceAop {

      @Around("execution(* com.miguel.hellospring..*(..))") // 이 AOP를 적용할 메서드들을 설정한다.
      public Object execute(ProceedingJoinPoint joinPoint) throws  Throwable {
          long start = System.currentTimeMillis();
          System.out.println("Start : " + joinPoint.toString());
          try {
              return joinPoint.proceed();
          } finally {
              long finish = System.currentTimeMillis();
              long timeMs = finish - start;
              System.out.println("End : " + joinPoint.toString() + " " + timeMs + "ms");
          }
      }
  }
```
&nbsp;
• **SpringConfig에 Bean으로 등록**
```
  @Bean
  public TimeTraceAop timeTraceAop() {
      return new TimeTraceAop();
  }
```
&nbsp;
&nbsp;
• **4. AOP 사용시 프로젝트 구조**
&nbsp;
![Aop Proxy](../../../../assets/images/AopProxy.png)
&nbsp;
- AOP를 사용하면, 실제 메서드가 아닌 프록시 메서드(...용도를 위한 가짜 메서드..?)를 만들어 원래 실행되어야 할 메서드 대신 실행시키고, 해당 프록시가 수행된 후 proceed()가 호출된 시점에 원래의 실제 메서드를 호출한다.

