---
emoji: ✨
title: 내일배움캠프) 키오스크 만들기
date: '2023-10-19 00:00:00'
author: 화나
tags: 내일배움캠프
categories: 내일배움캠프
---

> 자바 강의 완강 후 시작 된 개인 프로젝트! 키오스크 만들기이다. 1차 기능 구현은 완료했지만 아직 부족한 부분이 많이 보인다. 선택 요구사항을 구현하면서 차차 고쳐나가봐야지!

## Main.java

```java
public class Main {

    public static void main(String[] args){
        KioskApp app = new KioskApp();
        app.insertMenu();
        app.kiosk();
    }
}
```

- 프로젝트의 메인 클래스
- KioskApp 객체를 생성한 후 `insertMenu()` 메소드로 메뉴를 세팅하고, `kiosk()` 메소드로 키오스크를 실행했다.

## KioskApp.java

```java
import java.util.*;

public class KioskApp {

    private Map<Integer, List<Product>> allMenuMap = new HashMap<>(); // 전체 메뉴 지도
    private List<Menu> menuList = new ArrayList<>(); // 메뉴 리스트
    private List<Product> tteokbokkiList = new ArrayList<>(); // 떡볶이 리스트
    private List<Product> sideList = new ArrayList<>(); // 사이드 메뉴 리스트
    private List<Product> drinkList = new ArrayList<>(); // 음료 리스트
    private List<Product> mealKitList = new ArrayList<>(); // 밀키트 리스트
    Order order = new Order();

    public void insertMenu(){

        //메인 메뉴
        menuList.add(new Menu("Tteokbokki", "계속 생각나는 매운맛! 엽기떡볶이🥵"));
        menuList.add(new Menu("Side", "엽떡과 같이 먹으면 더 맛있어요🍙"));
        menuList.add(new Menu("Drinks", "매움을 달래주기 위한 음료🧃"));
        menuList.add(new Menu("Meal Kit", "어디서든 엽떡을 즐겨보세요🌳"));

        //떡볶이 메뉴
        tteokbokkiList.add(new Product("엽기떡볶이", "엽떡을 즐길줄 안다면 역시 오리지널!", 14000));
        tteokbokkiList.add(new Product("짜장떡볶이", "아이들이 먹기 좋아요", 16000));
        tteokbokkiList.add(new Product("로제떡볶이", "매운게 싫다면 부드러운 로제가 안성맞춤", 16000));
        tteokbokkiList.add(new Product("마라떡볶이", "전국품절 마라떡볶이! 재입고 되었습니다.", 16000));

        sideList.add(new Product("셀프 주먹김밥", "오물조물 만들어서 먹어요.", 2000));
        sideList.add(new Product("계란야채죽", "매운맛 소화기", 5000));
        sideList.add(new Product("순대", "떡볶이에 순대는 빠질수 없습니다.", 3000));
        sideList.add(new Product("야채튀김", "튀김도 마찬가지로 빠질수 없습니다.", 1000));

        drinkList.add(new Product("제로콜라", "살찌는게 걱정이라면 제로를 선택하세요.", 2000));
        drinkList.add(new Product("쿨피스", "매운걸 못먹는 분은 쿨피스 필수입니다.", 1000));

        mealKitList.add(new Product("오리지널맛", "엽떡을 즐길줄 안다면 역시 오리지널!", 15000));
        mealKitList.add(new Product("착한맛", "아이들이 먹기 좋아요", 15000));

        allMenuMap.put(1, tteokbokkiList);
        allMenuMap.put(2, sideList);
        allMenuMap.put(3, drinkList);
        allMenuMap.put(4, mealKitList);

    }

    public void kiosk(){

        while(true){
            int menuNum = printMenu(); //메인메뉴 출력

            if(menuNum == 6){ // 진행중인 주문 취소
                order.cancelOrder();
            }else if(menuNum == 5){
                if(order.printOrderList() == 1){
                    int waitingNum = order.getWaitingNum();

                    System.out.println("주문이 완료되었습니다.");
                    System.out.println("대기번호는 [" + waitingNum + " ]번 입니다.");
                    System.out.println("(3초 후 메뉴판으로 돌아갑니다.)");
                    order.setWaitingNum(waitingNum + 1);

                    try {
                        Thread.sleep(3000);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }else{
                int productNum = printMenu(menuNum); // 입력받은 숫자에 따른 상세 메뉴 출력
                Product selectProduct = allMenuMap.get(menuNum).get(productNum-1); //선택한 상품에 대한 정보 가져오기

                order.addProduct(selectProduct); // 카트에 담기
            }
        }
    }

    /**
     * 메인 메뉴 출력
     */
    public int printMenu(){

        System.out.println("🧡 엽기떡볶이에 오신걸 환영합니다. 🧡");
        System.out.println("아래 메뉴판을 보시고 메뉴를 골라 입력해주세요.");
        System.out.println();

        System.out.println("[ 🔥 YUPDDUCK MENU 🔥 ]");
        int index = 1;
        for(Menu m : menuList){
            System.out.print(index++ +". ");
            m.print();
        }

        System.out.println("[ 💛 ORDER MENU 💛 ]");
        System.out.print(index++ +". ");
        System.out.printf("%-15s | %s%n", "Order", "장바구니를 확인 후 주문합니다.⭕");
        System.out.print(index +". ");
        System.out.printf("%-15s | %s%n", "Cancel", "진행중인 주문을 취소합니다.❌");
        System.out.println();

        Scanner sc = new Scanner(System.in);
        return sc.nextInt();
    }

    /**
     * 상세 메뉴 출력
     */
    public int printMenu(int selectNum) {
        String menu = "TTEOKBOKKI";
        int index = 1;
        if(selectNum == 2){
            menu = "SIDE";
        }else if(selectNum == 3){
            menu = "DRINK";
        }else if(selectNum == 4){
            menu = "MEAL KIT";
        }

        System.out.println("엽기떡볶이에 오신걸 환영합니다.");
        System.out.println("아래 상품메뉴판을 보시고 상품을 골라 입력해주세요.");
        System.out.println();

        System.out.println("[ 🔥 "+ menu + " MENU 🔥 ]");
        //메뉴 리스트 출력
        for(Product p : allMenuMap.get(selectNum)){
            System.out.print(index++ +". ");
            p.print();
        }
        System.out.println();

        Scanner sc = new Scanner(System.in);
        return sc.nextInt();
    }
}
```

- 키오스크 실행 클래스, 메뉴를 출력하는 메소드를 오버로딩하여 사용했다.
- 인스턴스 변수
    - `allMenuMap` : 상세 메뉴 전체를 담는 리스트
    - `menuList` : 메인 메뉴를 담는 리스트
    - `tteokbokkiList` , `sideList` , `drinkList` , `mealKitList` : 각각의 상세메뉴를 담는 리스트
- 인스턴스 메소드
    - `insertMenu()` : 키오스크 실행 전, 메뉴를 세팅하는 메소드
    - `kiosk()` : 키오스크 실행 메소드
    - `printMenu()` : 메인 메뉴 출력 메소드
    - `printMenu(int selectNum)` : 상세 메뉴 출력 메소드

## Menu.java

```java
public class Menu {

    private String name;
    private String description;

    public Menu(String name, String description) {
        this.name = name;
        this.description = description;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }

    public void print(){
        System.out.printf("%-15s | %s%n",  getName(), getDescription());
    }
}
```

- 메인메뉴 클래스
- `print()` : 선택한 메뉴를 화면에 출력함

## Product.java

```java
public class Product extends Menu{

    public Product(String name, String description, int price) {
        super(name, description);
        this.price = price;
    }

    private int price;

    public int getPrice() {
        return price;
    }

    @Override
    public void print(){
        System.out.printf("%-15s | ₩ %s | %s%n",  super.getName(), getPrice(), super.getDescription());
    }
}
```

- 상세 메뉴 클래스
- `print()` : 상속받은 Menu의 `print()` 메소드를 재정의해서 사용함

## Order.java

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Order {
    private List<Product> orderList = new ArrayList<>();

    private int waitingNum;

    public Order() {
        this.waitingNum = 1;
    }

    public List<Product> getOrderList() {
        return orderList;
    }

    public void setWaitingNum(int waitingNum) {
        this.waitingNum = waitingNum;
    }

    public int getWaitingNum() {
        return waitingNum;
    }

    /**
     * 장바구니 담기
     * @param product : 사용자가 선택한 상품
     */
    public void addProduct(Product product) {
        product.print();
        System.out.println("위 메뉴를 장바구니에 추가하시겠습니까?");
        System.out.println("1. 확인      2. 취소");

        Scanner sc = new Scanner(System.in);
        int cartNum = sc.nextInt();

        if (cartNum == 1) {
            orderList.add(product);
            System.out.println("🛒 " + product.getName() + "가 장바구니에 추가되었습니다. 🛒");
        }else{
            System.out.println("❗ 취소되었습니다. ❗");
        }
    }

    /**
     * 주문내역, 총 가격 출력
     * @return
     */
    public int printOrderList(){
        System.out.println("아래와 같이 주문하시겠습니까?");
        System.out.println("[ Orders ]");

        int totalPrice = 0;

        for (Product product : getOrderList()) {
            totalPrice += product.getPrice();
            product.print();
        }

        System.out.println("[ Total Price ]");
        System.out.println(totalPrice);

        System.out.println("1. 주문     2. 메뉴판");

        Scanner sc = new Scanner(System.in);
        return sc.nextInt();
    }

    /**
     * 주문 취소
     */
    public void cancelOrder(){
        System.out.println("진행중이던 주문을 취소하시겠습니까?");
        System.out.println("1. 확인      2. 취소");

        Scanner sc = new Scanner(System.in);

        if(sc.nextInt() == 1){
            orderList.clear();
            System.out.println("진행중이던 주문이 취소되었습니다.");
        }
    }
}
```

- 주문 클래스, 주문에 관련된 메소드가 작성되어 있음
- `waitingNum` : 주문 객체 생성시 대기번호를 1로 초기화 함
- `addProduct(Product product)` : 선택된 상품을 장바구니에 담는 메소드
- `printOrderList()` : 주문 내역과 총 가격을 출력하는 메소드
    - 주문 내역에서 가격들을 가져와 총 가격을 계산한다.
- `cancelOrder()` : 주문 취소 메소드
    - 주문이 취소되면 장바구니를 비운다.
