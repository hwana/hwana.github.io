---
emoji: 💻
title: 완벽한 괄호
date: '2023-07-11 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

### 내 최종코드

```java
import java.util.*;
class Solution {
    boolean solution(String s) {

        int count = 0;

        for(int i = 0; i < s.length(); i++){
            if('(' == s.charAt(i)){
                count++;
            }else{
                count--;
            }

            if(count < 0){
                return false;
            }
        }

        return count == 0 ? true : false;
    }
}
```

- 처음에 매개변수를 String 형 배열로 만든 후 eqauls 함수를 써서 비교를 했더니 효율성 테스트에서 실패를 했다.
- 다른 사람의 코드를 참고하여 char로 비교를 했더니 효율성 테스트에서 통과되었다.

```toc

```
