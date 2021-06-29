---
layout: post
title:  "Java SpringBoot(14) - 회원 조회 기능 개발"
date:   2021-06-29 11:00:00 +0100
categories:
---

# Java SpringBoot(14) - 회원 조회 기능 개발
&nbsp;
&nbsp;
• **1. MemberController 코드**
&nbsp;
```
  // /src/java/hello.hellospring/controller/MemberController
  .
  .
  .
  @Controller
  public class MemberController {

      .
      .
      .

      @GetMapping("/members/")
      public String list(Model model) {
          List<Member> members = memberService.findMembers(); // 멤버 리스트 불러오기
          model.addAttribute("members", members); // model에 attribute를 추가
          return "members/memberList"; // members/memberList.html 리턴
      }


      @PostMapping("/members/new")
      public String create(MemberForm form) {
          Member member = new Member();
          member.setName(form.getName());

          memberService.join(member);

          return "redirect:/";
      }
  }
```
&nbsp;
&nbsp;
• **2. memberList.html 코드**
&nbsp;
```
  // /src/main/resources/templates/members/memberList.html
  <!DOCTYPE HTML>
  <html xmlns:th="http://www.thymeleaf.org">
  <body>
  <div class="container">
    <div>
      <table>
        <thead>
        <tr>
          <th>#</th>
          <th>이름</th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="member : ${members}"> // MemberController의 List 함수에서 값을 넣은 members List를 Loop 돈다.
          <td th:text="${member.id}"></td>
          <td th:text="${member.name}"></td>
        </tr>
        </tbody>
      </table>
    </div>
  </div> <!-- /container -->
  </body>
  </html>
```
&nbsp;
- thymeleaf 문법을 통해 members list의 값을 each로 loop 돈다. 