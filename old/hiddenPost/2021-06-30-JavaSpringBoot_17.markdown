---
layout: post
title: "Java SpringBoot(17) - JDBC Template"
date: 2021-06-30 11:00:00 +0100
categories:
---

# Java SpringBoot(17) - JDBC Template

&nbsp;
&nbsp;
• **1. Spring Jdbc Template**
&nbsp;

- 순수 Jdbc와 동일한 환경설정을 하면 된다.
- 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작생해야 한다.

&nbsp;
&nbsp;
• **2. Spring Jdbc Template 코드**
&nbsp;

```java
  package com.miguel.hellospring.repository;

  import com.miguel.hellospring.domain.Member;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.jdbc.core.JdbcTemplate;
  import org.springframework.jdbc.core.RowMapper;
  import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
  import org.springframework.jdbc.core.simple.SimpleJdbcInsert;

  import javax.sql.DataSource;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.HashMap;
  import java.util.List;
  import java.util.Map;
  import java.util.Optional;

  public class JdbcTemplateMemberRepository implements MemberRepository {

      private final JdbcTemplate jdbcTemplate;

      public JdbcTemplateMemberRepository(DataSource dataSource) {
          jdbcTemplate = new JdbcTemplate(dataSource);
      } // 생성자가 딱 하나만 있을 때, Autowired 생략 가능

      @Override
      public Member save(Member member) {
          SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
          jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

          Map<String, Object> parameters = new HashMap<>();
          parameters.put("name", member.getName());

          Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
          member.setId(key.longValue());
          return member;
      }

      @Override
      public Optional<Member> findById(Long id) {
          List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
          return result.stream().findAny();
      }

      @Override
      public Optional<Member> findByName(String name) {
          List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);
          return result.stream().findAny();
      }

      @Override
      public List<Member> findAll() {
          return jdbcTemplate.query("select * from member", memberRowMapper());
      }

      private RowMapper<Member> memberRowMapper() { // 리턴된 쿼리 결과를 member 객체 형태로 mapping
          return (rs, rowNum) -> {
              Member member = new Member();
              member.setId(rs.getLong("id"));
              member.setName(rs.getString("name"));
              return member;
          };
      }
  }
```
