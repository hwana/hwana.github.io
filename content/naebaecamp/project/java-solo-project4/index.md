---
emoji: ✨
title: 내일배움캠프) JAVA 개인 프로젝트 과제 구현 4차
date: '2023-10-25 00:00:00'
author: 화나
tags: 내일배움캠프
categories: 내일배움캠프
---

팀 과제에 추가된 요구사항을 구현했다. 간단한 부분을 맡게되어서 빨리 끝났다.

### 1. 요청사항 입력 기능 추가

```java
if ("1".equals(addProduct)) {
	String writeRequest = printQuestion("writeRequest");
	
	//요청 사항 입력 여부 확인
	if ("1".equals(writeRequest)) {
	    //요청사항이 조건에 맞을 때까지
	    while (true) {
	        System.out.println("요청 사항을 20자 이하로 입력해 주세요.");
	        Scanner sc = new Scanner(System.in);
	        String request = sc.nextLine();
	
	        if (checkRequestLength(request)) {
	            order.setRequest(request);
	            break;
	        }
	    }
    }
}

/**
 * 요청사항 길이 확인
 *
 * @param request : 입력 받은 요청 사항
 * @return : true : 조건 충족, false : 조건 불충족
 */
public boolean checkRequestLength(String request) {
    return request.length() <= 20;
}
```

- 요청사항은 필수가 아니기 때문에 입력한다고 선택할 경우에만 입력받도록 했다.
- 20자이하의 조건이 만족할 때까지 재요청을 한다.
- 조건을 만족한다면 order 인스턴스가 가지고 있는 request 필드에 저장한다.

### 2. 주문 확인 시 요청사항 출력

```java
System.out.println("[ 주문 요청사항 ]");
System.out.println(order.getRequest());
```

- order인스턴스가 가지고 있는 request를 가지고 온다.

### 3. 주문 상태 및 주문 상태 변경 기능 추가

```java
public boolean isStatus() {
    return status;
}

/**
 * 주문 완료 처리
 * @param status : 현재 주문 처리 상태
 */
public void switchStatus(boolean status) {
    this.status = !status;
}
```

- Order 클래스에 status 필드를 추가하고 false로 초기화했다.
- 주문 상태를 받아 변경해주는 switchStatus 메소드를 구현했다.