---
emoji: 💻
title: JadenCase 문자열 만들기
date: '2023-07-11 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

### 내 최종코드

```java
class Solution {
    public String solution(String s) {
        String[] arr = s.split("");
        String answer = arr[0].toUpperCase();

        for(int i = 1; i < arr.length; i++){
            if(" ".equals(arr[i])){
                answer += " ";
            }else{
                if(" ".equals(arr[i-1])){
                    answer += arr[i].toUpperCase();
                }else{
                    answer += arr[i].toLowerCase();
                }
            }
        }
        return answer;
    }
}
```

- 제일 첫 글자는 무조건 대문자이기 때문에 answer을 제일 첫글자로 초기화 해준다.
- for문을 돌면서 공백을 만난다면 answer을 공백을 더해준다.
- 공백이 아니라면 해당 글자의 앞이 공백인지 확인 후 앞 글자가 공백이면 대문자로 바꿔주고 아니라면 소문자로 바꿔준다.

### 다른 사람의 풀이

```java
class Solution {
  public String solution(String s) {
        String answer = "";
        String[] sp = s.toLowerCase().split("");
        boolean flag = true;

        for(String ss : sp) {
            answer += flag ? ss.toUpperCase() : ss;
            flag = ss.equals(" ") ? true : false;
        }

        return answer;
  }
}
```

- flag를 선언해서 공백을 판단한다.

```toc

```
