---
emoji: 💻
title: 의상
date: '2023-07-05 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

### 내 최종코드

```java
import java.util.*;
class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;

        HashMap<String, Integer> map = new HashMap<String, Integer>();
        for(int i = 0; i < clothes.length; i++){
            map.put(clothes[i][1], map.getOrDefault(clothes[i][1], 0) + 1);
        }

        for(String s : map.keySet()){
            answer *= map.get(s) + 1;
        }

        answer--;

        return answer;
    }
}
```

- (종류별 옷의 개수 + 1) \* (종류별 옷의 개수 + 1) - 1
- 종류별 옷의 개수에 1을 더해주는 이유는 해당 종류의 옷을 안입는 경우
- 다 곱해주고 -1을 빼는 이유는 모두 안입는 경우를 빼주기 위함임

```toc

```
