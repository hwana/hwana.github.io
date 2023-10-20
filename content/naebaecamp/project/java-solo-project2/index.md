---
emoji: ✨
title: 내일배움캠프) JAVA 개인 프로젝트 과제 구현 2차
date: '2023-10-20 00:00:00'
author: 화나
tags: 내일배움캠프
categories: 내일배움캠프
---



> 1차 과제 구현과 달라진 점
> 1. 예외처리 진행
> 2. 메뉴 초기화 방법 변경
> 3. 기능별로 메소드 분리하려고 노력함
> 4. 관리자 메뉴 추가
>
> 더이상 혼자 고민하는건 의미가 없다는 생각이 들어서 제출을 완료했다! 이제 피드백을 받고 내 코드에 무슨 문제가 있는지, 앞으로 어떻게 개선해 나가면 좋을지 생각해봐야겠다🥺

## Main.java

```java
public class Main {

    public static void main(String[] args) {
        KioskApp app = new KioskApp();
        while (true) {
            try {
                app.kiosk();
            } catch (Exception e) {
                System.out.println(e.getMessage());
            }
        }
    }
}
```

- KioskApp 객체를 생성할 때 insertMenu() 메소드를 실행하는 방향으로 바꾸었다.

## KioskApp.java

```java
import java.util.*;

public class KioskApp {

    private static final String NUMBER_REG = "^[1-6]*$";
    private Map<Integer, List<Product>> allMenuMap = new HashMap<>(); // 전체 메뉴 지도
    private List<Menu> menuList = new ArrayList<>(); // 메뉴 리스트
    private List<Product> tteokbokkiList = new ArrayList<>(); // 떡볶이 리스트
    private List<Product> sideList = new ArrayList<>(); // 사이드 메뉴 리스트
    private List<Product> drinkList = new ArrayList<>(); // 음료 리스트
    private List<Product> mealKitList = new ArrayList<>(); // 밀키트 리스트
    Order order = new Order();

    public KioskApp() {
        insertMenu();
    }

    public void insertMenu() {

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

    public void kiosk() throws Exception {

        String menuNum = printMenu(); //메인메뉴 출력
        Parser.parseNum(menuNum, NUMBER_REG);

        switch (menuNum) {
            case "6": // 주문 취소
                order.cancelOrder();
                break;
            case "5": // 주문하기
                if ("1".equals(order.orderCheck())) {
                    int waitingNum = order.getWaitingNum();

                    System.out.println("주문이 완료되었습니다.");
                    System.out.println("대기번호는 [" + waitingNum + " ]번 입니다.");
                    System.out.println("(3초 후 메뉴판으로 돌아갑니다.)");

                    //다음 주문을 위한 주문번호 세팅과 장바구니 초기화
                    order.setWaitingNum(waitingNum + 1);
                    order.getOrderList().clear();
                    order.setTotalPrice(0);

                    try {
                        Thread.sleep(3000);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                } else {
                    System.out.println("주문하지 않고 메뉴판으로 돌아갑니다.");
                }
                break;
            case "0": // 관리자 모드
                printAdmin();
                break;
            default: // 메뉴 선택
                String productNum = printMenu(menuNum); // 입력받은 숫자에 따른 상세 메뉴 출력
                Product selectProduct = allMenuMap.get(Integer.parseInt(menuNum)).get(Integer.parseInt(productNum) - 1); //선택한 상품에 대한 정보 가져오기

                order.addProduct(selectProduct); // 카트에 담기
        }
    }

    /**
     * 메인 메뉴 출력
     */
    public String printMenu() {

        System.out.println("🧡 엽기떡볶이에 오신걸 환영합니다. 🧡");
        System.out.println("아래 메뉴판을 보시고 메뉴를 골라 입력해주세요.");
        System.out.println();

        System.out.println("[ 🔥 YUPDDUCK MENU 🔥 ]");
        int index = 1;
        for (Menu m : menuList) {
            System.out.print(index++ + ". ");
            m.print();
        }

        System.out.println("[ 💛 ORDER MENU 💛 ]");
        System.out.print(index++ + ". ");
        System.out.printf("%-15s | %s%n", "Order", "장바구니를 확인 후 주문합니다.⭕");
        System.out.print(index + ". ");
        System.out.printf("%-15s | %s%n", "Cancel", "진행중인 주문을 취소합니다.❌");
        System.out.println();

        Scanner sc = new Scanner(System.in);
        return sc.nextLine();
    }

    /**
     * 상세 메뉴 출력
     */
    public String printMenu(String selectNum) {
        String menu = "TTEOKBOKKI";
        int index = 1;
        if ("2".equals(selectNum)) {
            menu = "SIDE";
        } else if ("3".equals(selectNum)) {
            menu = "DRINK";
        } else if ("4".equals(selectNum)) {
            menu = "MEAL KIT";
        }

        System.out.println("엽기떡볶이에 오신걸 환영합니다.");
        System.out.println("아래 상품메뉴판을 보시고 상품을 골라 입력해주세요.");
        System.out.println();

        System.out.println("[ 🔥 " + menu + " MENU 🔥 ]");
        //메뉴 리스트 출력
        for (Product p : allMenuMap.get(Integer.parseInt(selectNum))) {
            System.out.print(index++ + ". ");
            p.print();
        }
        System.out.println();

        Scanner sc = new Scanner(System.in);
        return sc.nextLine();
    }

    public void printAdmin() {
        System.out.println("[ 총 판매금액 현황 ]");
        System.out.println("현재까지 총 판매된 금액은 [ ₩ " + order.getAllTotalPrice() + " ] 입니다.");

        System.out.println("[ 총 판매상품 목록 현황 ]");
        System.out.println("현재까지 총 판매된 상품 목록은 아래와 같습니다.");

        for (String name : order.getAllOrderList().keySet()) {
            System.out.println("- " + name + " | ₩ " + order.getAllOrderList().get(name));
        }
    }
}

```

- `kiosk()` : if문보다 switch문이 가독성이 더 좋아보여 변경하였다.
- `printAdmin()` : 관리자의 메뉴를 출력하는 메소드를 추가하였다.

## Product.java

```java
public class Product extends Menu {

	public Product(String name, String description, int price) {
		super(name, description);
		this.price = price;
	}

	private int price;

	public int getPrice() {
		return price;
	}

	@Override
	public void print() {
		System.out.printf("%-15s | ₩ %s | %s%n", super.getName(), getPrice(),
			super.getDescription());
	}

	public void countPrint(int count) {
		System.out.printf("%-15s | ₩ %s | %s개 | %s%n", super.getName(), getPrice() * count, count,
			super.getDescription());
	}
}

```
- `countPrint()` : 개수에 따른 출력을 위해 추가하였다.

## Order.java

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class Order {

    private static final String YES_OR_NO = "^[1-2]*$";

    private Map<Product, Integer> orderList = new HashMap<>();    //주문 리스트
    private final Map<String, Integer> allOrderList = new HashMap<>(); //총 주문 리스트
    private int totalPrice;     //주문 별 총 판매금액
    private int allTotalPrice;    //전체 판매금액
    private int waitingNum;    //대기번호

    public Order() {
        this.waitingNum = 1;
    }

    public Map<Product, Integer> getOrderList() {
        return orderList;
    }

    public Map<String, Integer> getAllOrderList() {
        return allOrderList;
    }

    public int getTotalPrice() {
        return totalPrice;
    }

    public void setTotalPrice(int totalPrice) {
        this.totalPrice = totalPrice;
    }

    public int getAllTotalPrice() {
        return allTotalPrice;
    }

    public void setAllTotalPrice() {
        this.allTotalPrice += this.totalPrice;
    }

    public int getWaitingNum() {
        return waitingNum;
    }

    public void setWaitingNum(int waitingNum) {
        this.waitingNum = waitingNum;
    }


    /**
     * 장바구니 담기
     *
     * @param product : 사용자가 선택한 상품
     */
    public void addProduct(Product product) throws Exception {
        product.print();    //선택한 메뉴 출력
        String result = printQuestion("addProduct");   //선택 결과

        if ("1".equals(result)) {
            orderList.put(product, orderList.getOrDefault(product, 0) + 1);
            System.out.println("🛒 " + product.getName() + "가 장바구니에 추가되었습니다. 🛒");
        } else {
            System.out.println("❗ 취소되었습니다. ❗");
        }
    }

    /**
     * 주문 취소
     */
    public void cancelOrder() throws Exception {
        String result = printQuestion("cancelOrder");

        if ("1".equals(result)) {
            orderList.clear();
            setTotalPrice(0);
            System.out.println("진행중이던 주문이 취소되었습니다.");
        }
    }

    /**
     * 주문내역, 총 가격 출력
     *
     * @return
     */
    public String orderCheck() throws Exception {
        if (orderList.isEmpty()) {
            System.out.println("장바구니에 담긴 상품이 없습니다.");
            return "";
        }
        return printQuestion("orderCheck");
    }

    /**
     * 주문 품목 전체 리스트에 담기
     *
     * @param product
     */
    public void insertAllOrderList(Product product) {
        //전체 주문 목록에 판매한 품목과 품목별 금액 담기
        allOrderList.put(product.getName(), allOrderList.getOrDefault(product.getName(), product.getPrice()) + product.getPrice());
    }

    /**
     * 주문 건 별 총 금액 구하기
     *
     * @param product
     */
    public void setTotalPrice(Product product) {
        this.totalPrice += product.getPrice() * orderList.get(product);
    }


    /**
     * 선택지에 따른 질문 출력
     *
     * @param type
     * @return
     */
    public String printQuestion(String type) throws Exception {

        Scanner sc = new Scanner(System.in);
        String result = "";

        switch (type) {
            case "addProduct":  //장바구니 담기
                System.out.println("위 메뉴를 장바구니에 추가하시겠습니까?");
                System.out.println("1. 확인      2. 취소");
                result = sc.nextLine();
                break;
            case "cancelOrder": //주문 취소
                System.out.println("진행중이던 주문을 취소하시겠습니까?");
                System.out.println("1. 확인      2. 취소");
                result = sc.nextLine();
                break;
            case "orderCheck":  //주문 확인
                System.out.println("아래와 같이 주문하시겠습니까?");
                System.out.println("[ Orders ]");

                //장바구니에 동일한 품목이 2개이상일때만 개수 표현을 하기 위해서 최대값을 구함
                int maxCount = orderList.values().stream().max(Integer::compareTo).orElse(1);

                //주문 내역 출력
                for (Product product : getOrderList().keySet()) {
                    //장바구니에 각 품목이 한개씩만 담겼다면
                    if (maxCount == 1) {
                        product.print();    //개수 출력하지 않음
                    } else {
                        product.countPrint(orderList.get(product));  //1개가 아니라면 개수 출력
                    }

                    setTotalPrice(product);         //주문 건 별 총 금액 구하기
                    insertAllOrderList(product);    //주문 품목 전체 리스트에 담기
                }

                setAllTotalPrice(); //전체 주문 금액 구하기

                System.out.println("[ Total Price ]");
                System.out.println(totalPrice);

                System.out.println("1. 주문     2. 메뉴판");
                result = sc.nextLine();
        }
        Parser.parseNum(result, YES_OR_NO);
        return result;
    }
}

```
- `printQuestion()` : 사용자가 화면에서 선택한 값에 따라서 출력되는 부분을 하나로 묶은 메소드


## Parser.java
```java
import exception.BadInputException;

import java.util.regex.Pattern;

public class Parser {

    public static void parseNum(String number, String regex) throws Exception {
        if (!Pattern.matches(regex, number)) {
            throw new BadInputException();
        }
    }
}

package exception;

public class BadInputException extends Exception {
    public BadInputException() {
        super("잘못된 입력입니다! 화면에 출력된 숫자를 입력해주세요!");
    }
}

```
- 예외처리를 하기 위해 추가한 클래스
