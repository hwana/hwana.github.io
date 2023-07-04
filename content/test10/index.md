---
emoji: 💻
title: 폰켓몬
date: '2023-07-04 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/1845)

### 내 최종코드

```java
import java.util.*;
import java.util.stream.Collectors;
class Solution {
    public int solution(int[] nums) {
        int answer = 0;
        HashSet<Integer> set = new HashSet<Integer>();

        for(int i = 0; i < nums.length; i++){
            if(!set.contains(nums[i])){
                set.add(nums[i]);
            }
        }

        answer = nums.length / 2 > set.size() ? set.size() : nums.length / 2;

        return answer;
    }
}
```

```toc

```
