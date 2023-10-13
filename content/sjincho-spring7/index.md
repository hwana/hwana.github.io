---
emoji: 🐢
title: 과제7) Controller, Service, Repository
date: '2023-08-08 00:00:00'
author: 화나
tags: 스진초 자바
categories: 스진초 자바
---

스프링은 각 클래스를 빈으로 등록하기 위해서 `@Component`라는 어노테이션을 제공하지만, 실제 개발을 할 때는 `@Component`보다 `@Contoller`, `@Service`, `@Repository`를 더 많이 사용하게 되는 것 같다. 왜 `@Component`를 사용하지 않고 `@Component`의 하위개념인 어노테이션을 따로 제공하는걸까?

답을 찾기 위해 스프링 공식문서를 살펴봤더니 아래와 같은 문장이 있었다.

> Therefore, you can annotate your component classes with @Component, but, by annotating them with @Repository, @Service, or @Controller instead, your classes are more properly suited for processing by tools or associating with aspects.

`@Component`를 사용하여 클래스에 주석을 달 수 있지만 `@Contoller`, `@Service`, `@Repository`을 사용하는게 도구를 활용한 처리나 다양한 측면과의 연관성을 더 적절하게 갖출 수 있다고 나와있다. 너무 번역말투라 느낌가는대로 받아들이면 더 상세하게 분류해서 어노테이션을 달아주면 여러가지 상황에 적절하게 쓸 수 있다~ 이런 뜻이 아닐까?!🤔

각 계층에 따른 역할이 있고, 그 역할에 맞게 코드를 구현해 놓으면 유지보수 할 때 유연하게 대응할 수 있다는 생각이 든다! 그럼 각 계층에 역할에 대해서 알아보자.

## Controller(Presentation Layer)

- `@Contoller` 사용
- 사용자의 요청을 처리하고, 이에 따른 적절한 응답을 생성하는 역할

## Service(Service Layer)

- `@Service` 사용
- 비즈니스 로직을 구현하고 처리하는 부분
- 데이터 처리를 위해 레포지토리를 호출할 수 있음
- 컨트롤러와 데이터 액세스 계층 사이의 중간 역할

## Repository(Data Access Layer)

- `@Repository` 사용
- 데이터를 저장, 조회, 변경, 삭제하는등 데이터베이스 연상 수행
- 이 계층에서 데이터베이스와의 통신을 추상화하기 때문에 비즈니스 로직이 데이터와 분리되어 유지보수 성을 높일 수 있음

```toc

```
