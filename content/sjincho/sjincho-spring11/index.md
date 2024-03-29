---
emoji: 🐢
title: 과제11) 의존성 주입 방법
date: '2023-08-22 00:00:00'
author: 화나
tags: 스진초
categories: 스진초
---

## 생성자 주입

- 생성자를 통해서 의존관계를 주입 받는 방식
- 생성자 호출 시점에서 딱 한번만 호출되는 것이 보장되어 불변이나 필수 의존관계에 사용한다.
- 클래스에 생성자가 한개라면 `@Autowired`를 생략해도 자동주입이 된다.

## 수정자 주입(setter)

- 수정자 메소드를 통해서 의존관계를 주입하는 방식
- 선택이나 변경 가능성이 있는 의존관계에 주로 사용한다.

## 필드 주입

- 필드에 바로 주입하는 방법으로 외부에서 변경이 불가능하다.
- 잘 사용하지 않지만 테스트 코드에서 종종 사용하기도 한다.

### 생성자 주입을 사용해야 하는 이유

객체의 불변성을 확보해주기 때문이다. 실제로 개발을 하다보면 의존관계의 변경이 필요한 상황은 거의 없으므로 수정의 가능성을 열어둘 필요가 없다. 그러므로 생성자 주입을 통해서 변경의 가능성을 없애고 불변성을 보장하는 것이 좋다고 생각한다.

```toc

```
