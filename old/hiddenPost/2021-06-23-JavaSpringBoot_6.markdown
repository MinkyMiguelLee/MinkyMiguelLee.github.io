---
layout: post
title:  "Java SpringBoot(6) - API"
date:   2021-06-23 10:00:00 +0100
categories:
---

# Java SpringBoot(6) - API
&nbsp;
&nbsp;
• **1. API**
&nbsp;
- API는 별도 view나 template 없이, 데이터 그 자체를 주고받기 위해 사용된다.

&nbsp;
&nbsp;
• **2. API 예시**
```

  // controller
  package com.miguel.hellospring.controller;

  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RequestParam;
  import org.springframework.web.bind.annotation.ResponseBody;

  @Controller
  public class HelloController {
    
      @GetMapping("hello-string")
      @ResponseBody // http 통신 Response body에 return 값을 넣어주겠다는 것.
      public String helloString(@RequestParam("name") String name) {
          return "hello" + name;
      } // string을 리턴함. ex. hello 이민기

      @GetMapping("hello-api")
      @ResponseBody
      public Hello helloApi(@RequestParam("name") String name) {
          Hello hello = new Hello();
          hello.setName(name);
          return hello; // 객체를 반환하고, @ResponseBody 일 때엔 json으로 반환하는 것이 Default
      } // json 형태의 데이터를 리턴함. ex. {name: "이민기"}

      static class Hello {
          private String name;

          public String getName() {
              return name;
          }

          public void setName(String name) {
              this.name = name;
          }
      }
  }

```
&nbsp;
&nbsp;
![SpringBoot api structure](../../../../assets/images/api_structure.png)
&nbsp;
- @ResponseBody : http 통신 Response body에 return 값을 넣어주겠다는 것.
- 객체를 반환하고, @ResponseBody 일 때엔 json으로 반환하는 것이 Default
- viewResolver 대신 HttpMessageConverter가 동작
- 기본 문자처리 : StringHttpMessageConverter
- 기본 객체처리 : MappingJackson2HttpMessageConverter
- byte 처리 등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음.
