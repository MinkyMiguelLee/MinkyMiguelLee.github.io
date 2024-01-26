---
layout: post
title: "Java SpringBoot(13) - 회원 등록 기능 개발"
date: 2021-06-29 11:00:00 +0100
categories:
---

# Java SpringBoot(13) - 회원 등록 기능 개발

&nbsp;
&nbsp;
• **1. MemberController 코드**
&nbsp;

```java

  // /src/java/hello.hellospring/controller/MemberController
  .
  .
  .

  @Controller
  public class MemberController {

      .
      .
      .

      @GetMapping("/members/new") // home.html에 정의된 url.
      public String createForm() {
          return "members/createMemberForm"; // templates의 members/createMemberForm.html을 리턴한다.
      }

      // 회원 등록 페이지(/members/new)에서 등록 버튼을 눌렀을 때, post 방식으로 데이터를 넘겨주므로
      // Mapping 역시 GetMapping이 아닌 PostMapping으로 정의해야 한다.
      @PostMapping("/members/new")
      public String create(MemberForm form) { // MemberForm 객체를 넘겨받음
          Member member = new Member(); // Member를 만듦
          member.setName(form.getName()); // MemberForm을 통해 넘겨받은 이름을 getName으로 member 객체에 전달

          memberService.join(member); // memberService에 새 멤버를 등록

          return "redirect:/"; // 완료후 홈 화면으로 redirect
      }
  }

```

&nbsp;
&nbsp;
• **2. MemberForm 코드**
&nbsp;

```java

  // /src/java/hello.hellospring/controller/MemberForm
  package com.miguel.hellospring.controller;

  public class MemberForm {
      private  String name;

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
• **3. createMemberForm.html 코드**
&nbsp;

```html
<!-- /src/main/resources/templates/members/createMemberForm.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <form action="/members/new" method="post">
        <!-- form을 submit할 때, post형식으로 /members/new 호출 -->
        <div class="form-group">
          <label for="name">이름</label>
          <input
            type="text"
            id="name"
            name="name"
            placeholder="이름을 입력하세요"
          />
        </div>
        <button type="submit">등록</button>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```
