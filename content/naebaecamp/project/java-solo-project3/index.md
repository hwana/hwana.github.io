---
emoji: ✨
title: 내일배움캠프) JAVA 개인 프로젝트 과제 구현 3차
date: '2023-10-24 00:00:00'
author: 화나
tags: 내일배움캠프
categories: 내일배움캠프
---

1, 2차 과제 구현 게시글을 작성하면서 코드의 변화를 기록하기가 어려웠는데, 이번 게시글은 간단하게 작성하기로 하고 [깃 커밋내역](https://github.com/hwana/kiosk/commit/ed5138ddac55f9f0b7eb719569fa35dfbbdeaaf5)을 활용하기로 했다😗

3차 구현에서 수정된 부분은 아래와 같다.

## 1. OrderProcess 클래스 신규 생성

- 현재 상황 : Order 클래스에서 주문을 처리하기 위한 모든 작업을 진행했음
- 문제점 : 주문 인스턴스를 위한 멤버들과 주문을 처리하기 위한 멤버들이 혼잡하게 섞여있었다.
- 해결방안 : Order 클래스에서 주문을 처리하는 부분을 분리하여 OrderProcess 클래스로 신규 생성
- 결론 : 각각의 클래스가 본인의 역할만 수행하고 있게 된 것 같다.

## 2. 전체 주문 목록에 값이 잘못 들어가는 부분 수정

```java
//수정 전
allOrderList.put(product.getName(), allOrderList.getOrDefault(product.getName(), product.getPrice()) + product.getPrice());
//수정 후 
allOrderMap.put(product.getName(), allOrderMap.getOrDefault(product.getName(), 0) + product.getPrice() * order.getOrderMap().get(product));
```

- 문제점 : `getOrDefault(key, defaultValue)` 메소드에 기본값을 잘못 입력하여 전체 주문 목록에 값이 한번 더 더해져서 들어가는 상황 발생
- 해결방안 : 기본 값을 0으로 변경

## 3. 메소드의 분리

- 현재 상황 : 화면에 안내문구를 출력하는 메소드에서 관련 없는 작업(총 금액 구하기 등..)을 수행함
- 문제점 : 메소드의 역할이 모호해짐
    
    ```java
    //수정 전
    System.out.println("아래와 같이 주문하시겠습니까?");
    System.out.println("[ Orders ]");
    
    int maxCount = orderList.values().stream().max(Integer::compareTo).orElse(1);
    
    for (Product product : getOrderList().keySet()) {
        if (maxCount == 1) {
            product.print();    //개수 출력하지 않음
        } else {
            product.print(orderList.get(product));  //1개가 아니라면 개수 출력
        }
    
        setTotalPrice(product);         //주문 건 별 총 금액 구하기 : 관련 없는 작업
        insertAllOrderList(product);    //주문 품목 전체 리스트에 담기 : 관련 없는 작업
    }
    
    setAllTotalPrice(); //전체 주문 금액 구하기 : 관련 없는 작업
    
    System.out.println("[ Total Price ]");
    System.out.println(totalPrice);
    
    System.out.println("1. 주문     2. 메뉴판");
    ```
    
- 해결 방안 : 메소드 안에 관련 없는 작업은 따로 메소드로 분리하여 메소드의 역할과 책임을 구분함
    
    ```java
    //수정 후 : 관련 없는 메소드 제거
    System.out.println("아래와 같이 주문하시겠습니까?");
    System.out.println("[ Orders ]");
    
    //장바구니에 동일한 품목이 2개이상일때만 개수 표현을 하기 위해서 최대값을 구함
    int maxCount = order.getOrderMap().values().stream().max(Integer::compareTo).orElse(1);
    
    for (Product product : order.getOrderMap().keySet()) {
        //장바구니에 각 품목이 한개씩만 담겼다면
        if (maxCount == 1) {
            product.print();    //개수 출력하지 않음
        } else {
            product.print(order.getOrderMap().get(product));  //1개가 아니라면 개수 출력
        }
    }
    
    System.out.println("[ Total Price ]");
    System.out.println(order.getTotalPrice());
    
    System.out.println("1. 주문     2. 메뉴판");
    
    //관련 있는 작업끼리 모아서 메소드로 분리
    public void orderSuccess() {
    
        for (Product product : order.getOrderMap().keySet()) {
            // 전체 주문 목록에 사용자가 현재 주문한 목록 담기
            allOrderMap.put(product.getName(), allOrderMap.getOrDefault(product.getName(), 0) + product.getPrice() * order.getOrderMap().get(product));
        }
    
        // 전체 총 금액에 사용자가 현재 주문한 총 금액 담기
        setAllTotalPrice(order.getTotalPrice());
        //대기번호 생성
        int waitingNum = order.getWaitingNumber();
    
        System.out.println("주문이 완료되었습니다.");
        System.out.println("대기번호는 [" + waitingNum + " ]번 입니다.");
        System.out.println("(3초 후 메뉴판으로 돌아갑니다.)");
    
        //다음 주문을 위한 주문번호 세팅과 장바구니 초기화
        resetOrder();
    }
    ```