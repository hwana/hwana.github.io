---
emoji: 🐢
title: 과제3) @Override 꼭 적어야 할까요?
date: '2023-07-29 00:00:00'
author: 화나
tags: 스진초 자바
categories: 스진초 자바
---

2일차 과제를 제출하면서 강사님께 질문을 드렸다.
![사진](/content/spring3/q.png)

돌아온 대답은..!<br>
![사진](/content/spring3/a.png)

다 계획이 있으신 거였다😎

## 그래서 3일차의 과제는~!

> 1.  오버라이딩이 뭔가요?
> 2.  여러분들이 생각하실 때, 오버라이딩을 구현하려면 @Override를 꼭 적어야 하나요?

### 1. 오버라이딩이란?

하위 클래스가 상위 클래스의 메소드를 재정의해서 사용하는 것

### 2. 오버라이딩을 구현하려면 @Override를 꼭 적어야 하나요?

```java
class Parent {
    public void hello(String name) {
        System.out.println("안녕하세요, 저는 " + name + "입니다.");
    }
}

class Child extends Parent{
    // @Override가 있으면 어떻고 없으면 어떤가요?
    public void hello() {
        System.out.println("안녕!");
    }
}
```

#### 개발자가 hello라는 메소드를 어떻게 사용하느냐에 따라 달라진다.

1. Child 클래스의 hello 메소드로 사용하고 싶은 경우(오버로딩)
   - 이 경우에는 위 코드 그대로 사용해도 문제가 없다.
2. Parent 클래스의 hello 메소드를 사용하고 싶은 경우(오버라이딩)
   - 매개변수, 리턴타입, 메소드명을 상위 클래스의 메소드와 동일하게 작성한다면 `@Override` 없이도 오버라이딩된 메소드라고 인식이 된다.
   - 하지만 위 코드는 개발자의 실수로 매개변수가 다르게 작성되었다! 이때 `@Override`를 붙여준다면 컴파일 오류가 발생하게 되어 실수를 잡아낼 수가 있다.

> `@Override` 어노테이션은 필수는 아니지만, 작성을 해주면 컴파일 단계에서 개발자의 실수를 방지할 수 있다.

#### 참고 사진

##### 매개변수, 리턴타입, 메소드명을 동일하게 하고 `@Override`를 붙이지 않은 경우 : 컴파일 오류 없이 Parent 클래스의 hello 메소드를 override 한 것으로 인식된다.

![사진](/content/spring3/override.png)

##### 매개변수가 다른데 `@Override`를 붙인 경우 : 컴파일 오류가 발생한다.

![사진](/content/spring3/error.png)

```toc

```