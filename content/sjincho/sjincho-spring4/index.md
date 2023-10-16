---
emoji: 🐢
title: 과제4) 제어의 역전과 의존관계 주입
date: '2023-08-01 00:00:00'
author: 화나
tags: 스진초
categories: 스진초
---

제어의 역전과 의존관계 주입.. 영어로 보면 더 무슨 뜻인지 모르겠다. Inversion of Control, Dependency Injection 🙄 느에..?

두 용어에 대한 정의가 한번에 머리속에 들어오지는 않는다. 하지만 자바로 된 프로그램에 스프링을 적용하는 법을 배우다보면 어느새 은근슬쩍 내 머리에 들어와 있는 경험을 할 수 있다! 오늘은 [강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)를 들으면서 몸소 체험했던 IoC와 DI에 대해서 적어보려고 한다.(틀릴 수 있음 주의!🚨)

## 문제점

IoC와 DI가 적용되지 않은 프로그램은 아래와 같이 클라이언트 코드가 스스로 서버 객체를 생성하고 연결하고 실행했다. <br>

```java
class OrderServiceImpl implements OrderService{
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
}
```

수정이 필요할 경우 구현 객체를 클라이언트 코드에서 직접 수정해야 한다. 이럴경우 **객체지향적인 설계원칙을 위반**하게 된다. 그래서 클라이언트가 구현 객체와 상관없이 인터페이스에만 의존하도록 변경하여야 한다.

```java
class OrderServiceImpl implements OrderService{
    private final DiscountPolicy discountPolicy;
}
```

위 코드로 변경하면 객체지향적으로 설계한 것이지만 구현 객체가 없기 때문에 실행이 되지 않는다. 결국 구현 객체를 누군가 생성해서 대신 주입해주어야 한다.

## 해결방법

```java
//객체를 생성해서 주입해주는 역할
public class AppConfig{
    public OrderService orderService(){
        //OrderServiceImpl에 어떤 구현 객체를 주입할지는 여기에서 결정한다.
        return new OrderServiceImpl(new RateDiscountPolicy());
    }
}

public class OrderServiceImpl implements OrderService(){

    //인터페이스에만 의존
    private final DiscountPolicy discountPolicy;

    //생성자를 통해서 생성된 객체 주입 받음
    public OrderServiceImpl(DiscountPolicy discountpolicy){
        this.discountpolicy = discountpolicy;
    }
}
```

- `AppConfig`가 객체를 생성해서 주입해주기 때문에 `OrderServiceImpl`은 구현 객체를 신경쓰지 않고 실행에만 집중하면 된다.
- 이렇게 프로그램의 제어의 흐름을 클라이언트(OrderServiceImpl)가 직접 제어하는 것이 아니라 외부(AppConfig)에서 제어하는 것을 **제어의 역전**이라고 한다.
- 외부에서 실제 구현 객체를 생성하고 클라이언트에게 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것을 **의존관계 주입**이라고 한다.
- 의존관계가 주입되면 클라이언트측에서 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입을 변경할 수 있다.

## 스프링 적용하기

AppConfig를 사용해서 개발자가 직접 객체를 생성하고 의존관계 주입을 하던 것을 스프링에게 떠넘길수 있다😁

AppConfig에 `@Configuration` 어노테이션을 붙여주면 스프링 컨테이너는 AppConfig를 프로그램의 설정정보로 사용한다. AppConfig안에 있는 메소드들에 `@Bean` 어노테이션을 붙여주면 모두 스프링 컨테이너에 등록한다.

이제 스프링 컨테이너에서 스프링 빈으로 등록된 객체를 꺼내쓰면 된다.

여기까지 보면 '그래도 개발자가 설정정보에 객체를 반환하는 메소드를 만들어서 의존관계를 주입해줘야 하는거 아닌가?' 라는 생각이 들 수도 있다.

맞다. 지금까지는 AppConfig에 프로그램에서 사용할 객체를 반환하는 메소드를 작성해서 하나하나 스프링 컨테이너에 등록했지만, 갯수가 많아지면 관리하는데 한계가 있다.

그래서 스프링에서는 `@ComponentScan` 이라는 어노테이션을 제공한다. 설정정보 파일에 `@ComponentScan`를 붙이면 `@Conponent`가 붙은 클래스를 모두 빈으로 등록하고 이렇게 등록한 빈을 `@Autowired`를 활용해서 의존관계 주입을 해준다.

```java
@ComponentScan
public class AppConfig{

}
```

이렇게만 하면 된다.

> 내생각🙃 : 스프링에서 제공하는 어노테이션을 붙이면 프로그램의 흐름을 제어하면서 의존관계 주입까지 다 알아서 해준다.

```toc

```
