---
emoji: 💻
title: 신규 아이디 추천
date: '2023-07-03 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/72410)

### 내 최종코드

```java
class Solution {
    public String solution(String new_id) {
        String answer = new_id.toLowerCase();
        answer = answer.replaceAll("[^a-z0-9-_.]", "").replaceAll("[.]+", ".").replaceAll("^[.]|[.]$","");

        if("".equals(answer)) answer = "a";

        if(answer.length() >= 16){
            answer = answer.substring(0, 15);
        }

        answer = answer.replaceAll("[.]$", "");

        while(answer.length() <= 2){
            answer += answer.charAt(answer.length()-1);
        }

        return answer;
    }
}
```

```toc

```
