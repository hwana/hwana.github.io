---
emoji: 💻
title: 특별한 이차원 배열1
date: '2023-06-26 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/181833)

### 내 최종코드

```java
class Solution {
    public int[][] solution(int n) {
        int[][] answer = new int[n][n];

        for(int i = 0; i < answer.length; i++){
            for(int j = 0; j < answer.length; j++){
                answer[i][j] = i == j ? 1 : 0;
            }
        }
        return answer;
    }
}
```

### 다른 사람 풀이

```java
class Solution {
    public int[][] solution(int n) {
        int[][] answer = new int[n][n];
        for(int i = 0 ; i < n ; i++) {
            answer[i][i] = 1;
        }
        return answer;
    }
}
```

- i와 j가 같을 경우에만 1을 넣어주면 되기 때문에 위와 같은 코드로 작성하면 for문을 한 번만 작성해도 된다.

```toc

```
