---
emoji: 🐢
title: 과제8) 스프링에서 사용하는 어노테이션
date: '2023-08-11 00:00:00'
author: 화나
tags: 스진초 자바
categories: 스진초 자바
---

#### `@Component`

- 개발자가 작성한 클래스를 빈으로 등록하기 위한 어노테이션이다.

#### `@ComponentScan`

- `@Component`, `@Service`, `@Repository`, `@Controller`, `@Configuration`이 붙은 클래스를 찾아서 컨텍스트에 빈으로 등록해주는 어노테이션이다.

#### `@Bean`

- 개발자가 직접 제어가 불가능한 외부 라이브러리등을 빈으로 만들려할 때 사용되는 어노테이션이다.
- 외부 라이브러리 객체를 반환하는 메소드를 만들고 @Bean 어노테이션을 사용하면 된다.

#### `@Controller`

- 해당 클래스가 컨트롤러 역할을 한다는 것을 알려준다.

#### `@Service`

- 해당 클래스가 비즈니스 로직을 수행한다는 것을 의미한다.

#### `@Repository`

- 해당 클래스가 DB에 접근하는 메소드를 포함 한다는 것을 알려준다.

#### `@RequestParam`

- 요청 URI와 어노테이션에 작성된 value 값이 일치하면 해당 클래스가 실행된다.

#### `@RequestBody`

- HTTP 요청 바디를 자바 객체로 매핑해준다.

#### `@ResponseBody`

- 자바 객체를 HTTP 응답 바디로 매핑해준다.

#### `@Autowired`

- 필드, setter, 생성자에서 사용하며, 빈을 주입해준다.

```toc

```
