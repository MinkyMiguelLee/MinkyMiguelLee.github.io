---
layout: post
title:  "Java SpringBoot(12) - Web MVC 개발"
date:   2021-06-29 11:00:00 +0100
categories:
---

# Java SpringBoot(12) - Web MVC 개발
&nbsp;
&nbsp;
• **1. HomeController 코드**
&nbsp;
```
  // /src/java/hello.hellospring/controller/HomeController
  package com.miguel.hellospring.controller;

  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;

  @Controller
  public class HomeController {

      @GetMapping("/")
      public String home(){
          return "home";
      }

  }
```
&nbsp;
&nbsp;
• **2. home.html 코드**
&nbsp;
```
  // /src/main/resources/templates/home.html
  <!DOCTYPE HTML>
  <html xmlns:th="http://www.thymeleaf.org">
  <body>
      <div class = "container">
          <div>
              <h1>Hello Spring</h1>
              <p>회원 기능</p>
              <p>
                  <a href="/members/new">회원 가입</a>
                  <a href="/members">회원 목록</a>
              </p>
          </div>
      </div>
  </body>
  </html>
```
&nbsp;
&nbsp;
• **3. welcome page가 아닌 home.html이 보여지는 이유**
&nbsp;
![정적 컨텐츠](../../../../assets/images/homeMVC.png)
&nbsp;
- 위의 이미지에서 보여지듯, 먼저 스프링 컨테이너에서 관련된 컨트롤러가 있는지를 먼저 확인한 후, 없다면 static에서 정적 컨텐츠가 있는지를 확인하는 구조이다.
- 따라서, 이 경우에는 HomeController에서 정의한 home.html이 templates에 존재하는지를 먼저 확인한다. 이 때, templates에 원하는 파일이 있었기 때문에 home.html이 보여지게 된 것이다.

