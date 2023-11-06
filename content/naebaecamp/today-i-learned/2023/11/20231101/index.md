---
emoji: ✨
title: TIL) 다양한 의존관계 주입 방법
date: '2023-11-01 00:00:00'
author: 화나
tags: TIL
categories: TIL
---

## 📝 오늘의 내용정리
[알고리즘 스터디 4일차](https://github.com/StudySpringAlgorithm/Study_Algorithm_TeamSpring/blob/main/Kim/day4/day4.md)

### Math.sqrt()

제곱근을 구하는 함수

ex) sqrt(9) = 3

### Math.pow()

거듭제곱을 계산하는 함수

ex) pow(2, 3) = 8

### BeanFactory

- 스프링 컨테이너의 최상위 인터페이스이며 스프링 빈을 관리하고 조회하는 역할을 담당한다.

### ApplicationContext

- BeanFactory의 모든 기능을 상속 받아서 제공하며 부가기능이 더해진 컨테이너

### 다양한 의존관계 주입 방법

1. 생성자 주입
    - 생성자를 통해서 의존 관계를 주입 받는 방식
    - 생성자 호출 시점에서 딱 한번만 호출되는 것이 보장되어 불변이나 필수 의존관계에 사용한다.
    - 클래스에 생성자가 한개라면 `@Autowired`를 생략해도 자동 주입이 된다.
2. 수정자 주입(setter 주입)
    - 수정자 메소드를 통해서 의존관계를 주입하는 방법이며 선택이나 변경 가능성이 있는 의존관계에 사용한다.
3. 필드 주입
    - 필드에 바로 주입하는 방법으로 외부에서 변경이 불가능하기 때문에 테스트하기 힘들다는 치명적인 단점이 있다.
    - 사용하지 않는코드지만 애플리케이션의 실제 코드와 관계 없는 테스트 코드에서 종종 사용할 수도 있다.
4. 일반 메소드 주입
    - 일반 메소드를 통해서 주입 받을수 있으며 한번에 여러 필드를 주입 받을 수 있다.
    - 잘 사용하지 않는다.

#### 많은 자동 의존관계 주입 방법 중 생성자 주입을 권장한다.

##### 불변

대부분의 의존관계는 애플리케이션이을 한번 실행하고 나면 종료시점까지 의존관계를 변경할 일이 거의 없으며, 의존관계가 변하면 안된다.
수정자 주입을 사용하게 되면 setxxx메소드를 public으로 열어두어야 하기때문에 누군가 실수로 변경할 수도 있다.

##### 누락

생성자 주입을 사용하게 되면 의존관계 주입을 누락했을때 컴파일 오류가 발생하기 때문에 누락을 쉽게 알아챌 수 있다.

##### final

생성자 주입을 사용하게 되면 final 키워드를 사용할 수 있어서 생성자에 값이 설정되지 않는다면 컴파일 단계에서 막아준다.

> 참고<br>
생성자 주입을 제외한 나머지 주입방식은 모두 생성자 이후에 호출되므로 final 키워드를 사용할 수 없다.
생성자 주입과 수정자 주입을 동시에 사용해야 할 경우에는 기본으로 생성자 주입을 사용하고, 필수 값이 아닌 경우에 수정자 주입방식을 옵션으로 부여하면 된다.

```toc

```