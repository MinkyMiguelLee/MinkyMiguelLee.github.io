---
layout: post
title:  "Java SpringBoot(1)"
date:   2021-06-22 15:00:00 +0100
categories:
---

# Java SpringBoot(1) - 프로젝트 생성
&nbsp;
&nbsp;
• **1. SpringBoot Starter 사이트에서 스프링 프로젝트 생성**
[SpringBoot 스타터 페이지](https://start.spring.io/)

• **2. 프로젝트 생성**

• **프로젝트 선택**
    - Project : Gradle Project
    - Spring Boot : 2.3.x
    - Language : Java
    - Packaging : Jar
    - Java : 11

• **프로젝트 메타데이터**
    - Group
    - Artifact - Build 되어 나온 결과물...즉 프로젝트 명

• **Dependencies**
    - Spring Web, Thymeleaf(Template Engine)
    - Gardle 이나 Maven과 같은 Build Tool은 의존관계를 자동으로 관리해 준다. 특정 라이브러리를 가져왔을 때, 해당 라이브러리와 의존관계를 가지는 라이브러리를 자동으로 가져온다.

• **3. 생성된 프로젝트를 IntelliJ에서 Import**

&nbsp;
&nbsp;
![SpringBootFileStructure](../../../../assets/images/SpringBoot_Structure.png)