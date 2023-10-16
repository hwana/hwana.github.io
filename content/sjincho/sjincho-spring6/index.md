---
emoji: 🐢
title: 과제6) Entity, VO, DTO
date: '2023-08-04 00:00:00'
author: 화나
tags: 스진초
categories: 스진초
---

### DTO(Data Transfer Object)

- 데이터 이동 객체라는 의미를 갖는다.
- 계층 간 데이터를 주고 받을 때 사용하며 로직을 갖지않는 순수한 데이터 객체이다.
- 데이터를 어떤 방식으로 초기화하느냐에 따라서 가변객체/불변객체로 구분된다.

#### 가변객체 : setter로 데이터를 초기화하는 경우

```java
class Person{
    private String name;

    public String getName(){
        return name;
    }

    public void setName(){
        this.name = name;
    }
}
```

#### 불변객체 : 생성자로 데이터를 초기화 하는 경우

```java
class Person{
    private String name;

    //생성자
    public Person(String name){
        this.name = name;
    }

    public String getName(){
        return name;
    }
}
```

### VO(Value Object)

- 값 자체를 표현하는 객체
- 객체의 불변성을 보장하고 값을 읽는 것만 가능하다.
- 서로 다른 이름을 가진 VO더라도 모든 속성 값이 같다면 두 인스턴스는 같은 객체라고 할 수 있다. 이를 보장하기 위해서는 Object 클래스의 `equals()` 와 `hashCode()`를 오버라이딩 해야한다.

### Entity

- 데이터베이스 테이블과 1:1로 매핑되는 클래스로 DB 테이블 내의 컬럼만 필드로 가져야 한다.
- Entity를 기준으로 테이블이 생성되고 스키마가 변경되기 때문에 요청이나 응답값을 전달하는 클래스로 사용하면 안된다.

```java
import javax.persistence.*;

@Entity
public class User {
   @Id
   @GeneratedValue(strategy = GenerationType.IDENTITY)
   privae Long id;

   @Column(nullable = false)
   private String name;

   @Column(nullable = false)
   private String email;

   @Builder
   public User(String name, String email) {
      this.name = name;
      this.email = email;
   }

   public User update(String name, String email) {
      this.name = name;
      this.email = email;
      return this;
   }
}
```

### 비교하기

| DTO                                 | VO                      | Entity                         |
| ----------------------------------- | ----------------------- | ------------------------------ |
| 레이어 간 데이터 전송만을 위한 객체 | 값을 표현하기 위한 객체 | DB 테이블을 매핑하기 위한 객체 |
| 가변성, 읽고 쓰는 것 모두 가능      | 불변성, 읽는것만 가능   | 가변성, 읽고 쓰는 것 모두 가능 |
| 로직을 포함할 수 없음               | 로직을 포함할 수 있음   | 로직을 포함할 수 있음          |

[출처1](https://devmoony.tistory.com/180)
[출처2](https://youngjinmo.github.io/2021/04/dto-vo-entity/)

```toc

```
