---
emoji: 💻
title: 신고 결과 받기
date: '2023-06-25 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/92334)

### 내 최종코드

```java
import java.util.*;
class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
        int[] answer = new int[id_list.length]; //결과값 반환 배열
        HashSet<String> hashSet = new HashSet<>(Arrays.asList(report)); //신고 결과에서 중복값을 제거한 hastSet

        //신고받은 결과
        Map<String, Integer> complaintMap = new HashMap<String, Integer>();
        for(String key : hashSet){
            String id = key.split(" ")[1]; //신고받은 id
            complaintMap.put(id, complaintMap.getOrDefault(id, 0) + 1); //신고받은 횟수
        }

        //신고한 결과
        Map<String, Integer> resultMap = new HashMap<String, Integer>();
        for(String key : hashSet){
            String fromId = key.split(" ")[0]; //신고한 id
            String toId = key.split(" ")[1]; //신고받은 id

            if(complaintMap.get(toId) >= k){    //신고받은 횟수가 k회 이상이라면
                resultMap.put(fromId, resultMap.getOrDefault(fromId, 0) + 1);
            }
        }

        int index = 0;
        //id를 담고있는 배열 순서대로 answer에 결과값 넣기
        for(String id : id_list){
            answer[index++] = resultMap.getOrDefault(id, 0);
        }

        return answer;
    }
}
```

```toc

```
