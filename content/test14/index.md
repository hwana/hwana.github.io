---
emoji: 💻
title: 다음 큰 숫자
date: '2023-07-17 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/12911)

### 내 최종코드

```java
class Solution {
    public int solution(int n) {

        int answer = n + 1;
        int nowCount = getCount(n);

        while(true){
            int plusCount = getCount(answer);

            if(nowCount == plusCount){
                break;
            }else{
                answer++;
            }
        }

        return answer;
    }

    int getCount(int n){
        String binary = Integer.toBinaryString(n);
        char[] arr = binary.toCharArray();
        int count = 0;

        for(char c : arr){
            if('1' == c) count++;
        }

        return count;
    }

}
```

- 주어진 숫자 n을 이진수로 바꿔서 String형 배열에 넣었는데 시간초과 오류가 떴다.
- 이전 문제처럼 char형 배열로 변경해주었더니 통과되었다.

```toc

```
