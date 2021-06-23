---
layout: post
title:  "Java SpringBoot(5) - MVC와 Template engine"
date:   2021-06-23 10:00:00 +0100
categories:
---

# Java SpringBoot(5) - MVC와 Template engine
&nbsp;
&nbsp;
• **1. MVC?**
&nbsp;
- Model, View, Controller
- 과거에는 View와 Controller가 분리되어 있지 않았다(model 1 방식)
- 최근 개발 트렌드는 위와 다름. 각각의 역할을 분리하여 개발한다.
  - Controller는 비즈니스 로직 관련 / 내부 처리 등을 전담
  - View는 화면을 그리는 데 집중
  - Model이 화면 구성에 필요한 데이터를 담아 View에 전달

&nbsp;
&nbsp;
• **컨트롤러 예시**
```
  package com.miguel.hellospring.controller;

  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RequestParam;

  @Controller
  public class HelloController {
      @GetMapping("hello-mvc")
      public String helloMvc(@RequestParam("name") String name, Model nodel){
        // @RequestParam으로 이 컨트롤러가 전달받을 파라미터를 정의할 수 있다.
        // default로 required === true임.
          nodel.addAttribute("name", name);
          return "hello-template";
      }
  }
```
&nbsp;
&nbsp;
• **템플릿 예시**
```
  // hello-template.html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
    <p th:text="'안녕하세요. ' + ${name}">안녕하세요. 손님.</p>
  </body>
  </html>
```
&nbsp;
&nbsp;
• **2. 작동 과정**
&nbsp;
- url에 get 방식으로 컨트롤러에 정의한것과 같이 데이터를 전달한다(http://localhost:8080/hello-mvc?name=이민기).
- 컨트롤러에서 name에 파라미터로 전달해 준 값이 들어가게 되고, 이 name을 model이 들고 template에 전달.
- template에서 ${name} 형태로 model에 들어있는 name 값을 꺼내어 쓴다. 

&nbsp;
![SpringBoot mvc structure](../../../../assets/images/mvc_structure.png)
