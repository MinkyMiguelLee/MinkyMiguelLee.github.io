---
layout: post
title:  "Java SpringBoot(5) - MVC와 Template engine"
date:   2021-06-23 10:00:00 +0100
categories:
---

# Java SpringBoot(4) - MVC와 Template engine
&nbsp;
&nbsp;
• **1. 정적 컨텐츠**
&nbsp;
- 서버에서의 별도의 조작 없이 이미 존재하는 파일이나 그 내용을 그대로 웹 브라우저에 전달하는 것.
- 스프링부트는 정적 컨텐츠 기능을 제공한다.
- ex) static 폴더에 ex.html을 정의해 둔 후, http://localhost:8080/ex.html로 접근하면 html에 정의된 내용이 그대로 보여진다.

• **2. 정적 컨텐츠 반환의 구조**
&nbsp;
![SpringBoot Static contents structure](../../../../assets/images/static_structure.png)