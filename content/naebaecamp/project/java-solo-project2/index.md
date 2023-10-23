---
emoji: ✨
title: 내일배움캠프) JAVA 개인 프로젝트 과제 구현 2차
date: '2023-10-20 00:00:00'
author: 화나
tags: 내일배움캠프
categories: 내일배움캠프
---

> 더이상 혼자 고민하는건 의미가 없다는 생각이 들어서 제출을 완료했다! 이제 피드백을 받고 내 코드에 무슨 문제가 있는지, 앞으로 어떻게 개선해 나가면 좋을지 생각해봐야겠다🥺
>
> ### [1차 과제 구현](https://hwana.github.io/naebaecamp/project/java-solo-project/)과 달라진 점
> 1. 예외처리 진행
> 2. 메뉴 초기화 방법 변경
> 3. 기능별로 메소드 분리하려고 노력함
> 4. 관리자 메뉴 추가

## Main.java

```java
public class Main {

    public static void main(String[] args) {
    
        KioskApp app = new KioskApp();

        /* 변경 전
        app.insertMenu();
        app.kiosk();
        */

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
- insertMenu()는 앱이 시작될 때 항상 실행되어야 하는 메소드로, KioskApp 객체를 생성할 때 실행되도록 하였다.
- 예외처리를 진행하여 호출 된 메소드들이 예외를 던질 때 예외 메시지를 출력하도록 했다.

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

        menuList.add(new Menu("Tteokbokki", "계속 생각나는 매운맛! 엽기떡볶이🥵"));
        tteokbokkiList.add(new Product("엽기떡볶이", "엽떡을 즐길줄 안다면 역시 오리지널!", 14000));   
        sideList.add(new Product("셀프 주먹김밥", "오물조물 만들어서 먹어요.", 2000));
        drinkList.add(new Product("제로콜라", "살찌는게 걱정이라면 제로를 선택하세요.", 2000));
        mealKitList.add(new Product("오리지널맛", "엽떡을 즐길줄 안다면 역시 오리지널!", 15000));
        //..중략

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
                Parser.parseNum(productNum, NUMBER_REG);
                Product selectProduct = allMenuMap.get(Integer.parseInt(menuNum)).get(Integer.parseInt(productNum) - 1); //선택한 상품에 대한 정보 가져오기

                order.addProduct(selectProduct); // 카트에 담기
        }
    }

    //..중략

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
- 각각의 입력이 숫자가 아닐 경우 오류를 던지게 수정
- `kiosk()` : if문보다 switch문이 가독성이 더 좋아보여 변경하였다.
- `printAdmin()` : 관리자의 메뉴를 출력하는 메소드를 추가하였다.

## Product.java

```java
public class Product extends Menu {

	//..중략

	public void print(int count) {
		System.out.printf("%-15s | ₩ %s | %s개 | %s%n", super.getName(), getPrice() * count, count,
			super.getDescription());
	}
}

```
- `print(int count)` : 개수에 따른 출력을 위해 추가하였다.

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

    //..중략


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
- `private Map<Product, Integer> orderList` : 개수 출력을 위해 List에서 Map으로 변경
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
