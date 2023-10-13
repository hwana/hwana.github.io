---
emoji: 📚
title: 다형성
date: '2023-06-20 00:00:00'
author: 화나
tags: 자바 자바의정석
categories: 자바 자바의정석
---

## 다형성

상위 클래스 타입의 참조변수로 하위 클래스의 인스턴스를 참조할 수 있도록 하는 것

```java
class Tv{
	boolean power;
	int channel;

	void power(){
		power = !power;
	}

	void channelUp(){
		++channel;
	}

	void channelDown(){
		--channel;
	}
}

class CaptionTv extends Tv{
	String text;
	void Caption(){}
}
```

- 두 클래스가 상속관계에 있을 때 상위 클래스 타입의 참조변수로 하위 클래스의 인스턴스를 참조하도록 할 수 있다.
  - `Tv t = new CaptionTv();`
  - 참조변수 t로는 Tv클래스의 멤버들만 사용할 수 있다.
  - 참조변수의 타입이 참조변수가 참조하고 있는 인스턴스에서 사용할 수 있는 멤버의 갯수를 결정한다.

### 참조변수와 인스턴스의 연결

```java
class BindingTest{
	public static void main(String[] args) {
		Parent p = new Child();
		Child c = new Child();

		System.out.println("p.x = " + p.x);
		p.method();

		System.out.println("c.x = " + c.x);
		c.method();
	}
}

class Parent {
	int x = 100;

	void method() {
		System.out.println("Parent Method");
	}
}

class Child extends Parent {
	int x = 200;

	void method() {
		System.out.println("Child Method");
	}
}
```

- 메소드의 경우 실제 인스턴스의 메소드가 호출되지만 멤버변수의 경우 참조변수 타입에 따라 달라진다. 참조변수의 타입에 따라 결과가 달라지는 경우는 상위 클래스의 멤버변수와 같은 이름의 멤버변수를 하위클래스에 중복해서 정의한 경우 뿐이다.
- 메소드인 method()의 경우 참조변수의 타입에 관계없이 항상 실제 인스턴스 타입인 Child 클래스에 정의된 메소드가 호출되지만, 인스턴스변수인 x의 경우 참조변수 타입에 따라서 달라진다.

```java
class BindingTest2 {
	public static void main(String[] args) {
		Parent p = new Child();
		Child c = new Child();

		System.out.println("p.x = " + p.x);
		p.method();

		System.out.println("c.x = " + c.x);
		c.method();
	}
}

class Parent {
	int x = 100;

	void method() {
		System.out.println("Parent Method");
	}
}

class Child extends Parent { }
```

- 해당 예시는 Child 클래스에 아무런 멤버도 정의되어 있지 않기 때문에 참조변수 타입이 다르더라도 항상 상위 클래스의 멤버를 사용하게 된다.

### 매개변수의 다형성

```java
class Product {
	int price;			// 제품의 가격
	int bonusPoint;		// 제품구매 시 제공하는 보너스점수

	Product(int price) {
		this.price = price;
		bonusPoint =(int)(price/10.0);	// 보너스점수는 제품가격의 10%
	}
}

class Tv extends Product {
	Tv() {
		// 조상클래스의 생성자 Product(int price)를 호출한다.
		super(100);			// Tv의 가격을 100만원으로 한다.
	}

	public String toString() {	// Object클래스의 toString()을 오버라이딩한다.
		return "Tv";
	}
}

class Computer extends Product {
	Computer() {
		super(200);
	}

	public String toString() {
		return "Computer";
	}
}

class Buyer {			// 고객, 물건을 사는 사람
	int money = 1000;	// 소유금액
	int bonusPoint = 0;	// 보너스점수

	**void buy(Product p) {**
		if(money < p.price) {
			System.out.println("잔액이 부족하여 물건을 살수 없습니다.");
			return;
		}

		money -= p.price;			// 가진 돈에서 구입한 제품의 가격을 뺀다.
		bonusPoint += p.bonusPoint;	// 제품의 보너스 점수를 추가한다.
		System.out.println(p + "을/를 구입하셨습니다.");
	}
}

class PolyArgumentTest {
	public static void main(String args[]) {
		Buyer b = new Buyer();

		**b.buy(new Tv());
		b.buy(new Computer());**

		System.out.println("현재 남은 돈은 " + b.money + "만원입니다.");
		System.out.println("현재 보너스점수는 " + b.bonusPoint + "점입니다.");
	}
}
```

- 매개변수가 Product타입의 참조변수라는 것은, 메소드의 매개변수로 Product클래스의 하위 클래스 타입의 참조변수면 어느 것이나 매개변수를 받아들일 수 있다는 뜻이다.

```toc

```
