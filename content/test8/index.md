---
emoji: 💻
title: 개인정보 수집 유효기간
date: '2023-06-27 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/150370)

### 내 최종코드

```java
import java.util.*;
class Solution {
    public int[] solution(String today, String[] terms, String[] privacies){
        int[] answer = {};
        ArrayList<Integer> list = new ArrayList<Integer>();
        Map<String, Integer> termsMap = new HashMap<String, Integer>();

        for(String s : terms){
            termsMap.put(s.split(" ")[0], Integer.parseInt(s.split(" ")[1]) * 28);
        }

        for(int i = 0; i < privacies.length; i++){
            if(!minus(privacies[i], today, termsMap)){
                list.add(i+1);
            }
        }

        answer = new int[list.size()];
        for(int i = 0; i < list.size(); i++){
            answer[i] = list.get(i);
        }

        return answer;
    }

    //각 날짜의 일수 차이를 구하는 메소드
    public boolean minus(String privacies, String today, Map<String, Integer> termsMap){
        String pDate = privacies.split(" ")[0];
        String alpha = privacies.split(" ")[1];

        //유효기간이 지났을 경우 false 리턴
        if(getDate(today) - getDate(pDate) >= termsMap.get(alpha)){
            return false;
        }
        return true;
    }

    //주어진 날짜를 일수로 구하는 메소드
    public int getDate(String date){
        String[] arr = date.split("\\.");
        int year = Integer.parseInt(arr[0]);
        int month = Integer.parseInt(arr[1]);
        int day = Integer.parseInt(arr[2]);

        return (year * 12 * 28) + (month * 28) + day;
    }
}
```

```toc

```
