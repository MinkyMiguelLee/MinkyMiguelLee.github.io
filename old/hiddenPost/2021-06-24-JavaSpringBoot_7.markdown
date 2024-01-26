---
layout: post
title: "Java SpringBoot(7) - 테스트케이스"
date: 2021-06-24 10:00:00 +0100
categories:
---

# Java SpringBoot(7) - 테스트케이스

&nbsp;
&nbsp;
• **1. JUnit**
&nbsp;

- 개발한 기능을 실행해서 테스트 할 때, 자바의 main 메서드를 실행하거나 웹 어플리케이션의 컨트롤러를 실행하여 해당 기능을 테스트한다. 이러한 방법은 준비 및 실행에 오랜 시간이 걸리고, 반복 실행이 어려우며 여러 테스트를 한 번에 실행하기 어렵다는 단점이 있다.
- 이러한 단점을 보완하기 위해 자바는 JUnit이라는 프레임워크로 테스트를 실행한다.

&nbsp;
&nbsp;
• **2. 회원관리 도메인 코드**

```java

  // /domain/Member
  package com.miguel.hellospring.domain;

  public class Member {

      private Long id; // 회원 ID
      private String name; // 회원 이름

      public Long getId() {
          return id;
      }

      public void setId(Long id) {
          this.id = id;
      }

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
• **3. 회원관리 repository 코드**

```java

  // Interface - MemberRepository
  package com.miguel.hellospring.repository;

  import com.miguel.hellospring.domain.Member;

  import java.util.List;
  import java.util.Optional;

  public interface MemberRepository {
      Member save(Member member);
      Optional<Member> findById(Long id);
      Optional<Member> findByName(String name);
      List<Member> findAll();

  }

```

&nbsp;

```java

  // class - MemoryMemberRepository
  package com.miguel.hellospring.repository;

  import com.miguel.hellospring.domain.Member;

  import java.util.*;

  public class MemoryMemberRepository implements MemberRepository{

      private  static Map<Long, Member> store = new HashMap<>();
      private  static  long sequence = 0L;

      @Override
      public Member save(Member member) {
          member.setId(++sequence);
          store.put(member.getId(), member);
          return member;
      }

      @Override
      public Optional<Member> findById(Long id) {
          return Optional.ofNullable(store.get(id));
      }

      @Override
      public Optional<Member> findByName(String name) {
          return store.values().stream().filter(member -> member.getName().equals(name)).findAny();
      }

      @Override
      public List<Member> findAll() {
          return new ArrayList<>(store.values());
      }
  }

```

&nbsp;
&nbsp;
• **4. 회원관리 repository - Test 코드**

```java

  package com.miguel.hellospring.repository;

  import com.miguel.hellospring.domain.Member;
  import org.junit.jupiter.api.AfterEach;
  import org.junit.jupiter.api.Test;

  import java.util.List;

  import static org.assertj.core.api.Assertions.*;

  public class MemoryMemberRepositoryTest {
      MemoryMemberRepository repository = new MemoryMemberRepository();

      // 테스트 메서드의 실행 순서는 보장되지 않는다. 따라서 메서드별로 순서와 무관하게 따로 수행되도록 설계하여야 한다

      @AfterEach // 각각의 테스트 메서드가 수행된 후 매번 이 메서드가 실행된다.
      public void afterEach(){
          // 이 경우 매 수행마다 repository를 비운다. 따라서 테스트 메서드들이 repository에 들어있는 다른
          // 데이터를 신경쓰지 않아도 된다.
          repository.clearStore();
      }

      @Test // JUnit을 import 한다. 이루 해당 메서드를 직접 실행할 수 있다.
      public void save() {
          // 유저정보 저장 기능 테스트케이스
          Member member = new Member();
          member.setName("Spring");

          repository.save(member);

          Member result = repository.findById(member.getId()).get();

          assertThat(member).isEqualTo(result); // 두 값이 같다면 테스트케이스 수행 성공, 다르면 실패
      }

      @Test
      public void findByName() {
          Member member1 = new Member();
          member1.setName("spring1");
          repository.save(member1);

          Member member2 = new Member();
          member2.setName("spring2");
          repository.save(member2);

          Member result = repository.findByName("spring1").get();

          assertThat(result).isEqualTo(member1);
      }

      @Test
      public void findAll() {
          Member member1 = new Member();
          member1.setName("spring1");
          repository.save(member1);

          Member member2 = new Member();
          member2.setName("spring2");
          repository.save(member2);

          List<Member> result = repository.findAll();

          assertThat(result.size()).isEqualTo(2);
      }
  }

```
