---
layout: post
title: "Java SpringBoot(8) - Service 개발"
date: 2021-06-24 10:00:00 +0100
categories:
---

# Java SpringBoot(8) - Service 개발

&nbsp;
&nbsp;
• **1. Service**
&nbsp;

- 앞에서 구현한 repository를 기반으로 한 비즈니스 로직 개발

&nbsp;
&nbsp;
• **2. Service 코드**

```

  // /service/MemberService
  package com.miguel.hellospring.service;

  import com.miguel.hellospring.domain.Member;
  import com.miguel.hellospring.repository.MemberRepository;
  import com.miguel.hellospring.repository.MemoryMemberRepository;

  import java.util.List;
  import java.util.Optional;

  public class MemberService {

      private final MemberRepository memberRepository = new MemoryMemberRepository();

      // 회원 가입
      public Long join(Member member){
          // 같은 이름의 중복 회원 가입 불가
          validateDuplicatedMember(member); // 회원 중복여부 검증
          memberRepository.save(member);
          return member.getId();
      }

      private void validateDuplicatedMember(Member member) {
          memberRepository.findByName(member.getName()) // Optional<Nember> 로 리턴됨.
              .ifPresent(m->{ // Optional 내장 메서드...
              throw new IllegalStateException("이미 존재하는 회원이다");
          });
      }

      // 전체 회원 조회
      public List<Member> findMembers() {
          return memberRepository.findAll();
      }

      // id로 단일 회원 조회
      public Optional<Member> findOne(Long memberId) {
          return memberRepository.findById(memberId);
      }

  }

```

&nbsp;
&nbsp;
• **3. 단축키**

- Command + Option + v : 자동으로 리턴할 변수를 만들어줌
- Ctrl + t : 리팩토링과 관련된 기능들을 사용할 수 있음. (ex. 어떤 기능을 메서드로 extract...)
- Command + shift + t : 새로운 테스트코드 생성
