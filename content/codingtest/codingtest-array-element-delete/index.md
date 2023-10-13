---
emoji: 💻
title: 배열의 원소 삭제하기
date: '2023-06-26 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/181844)

### 내 최종코드

```java
import java.util.*;
class Solution {
    public int[] solution(int[] arr, int[] delete_list) {
        int[] answer = {};
        ArrayList<Integer> list = new ArrayList<Integer>();

        for(int a : arr){
            list.add(a);
        }

        for(int a : delete_list){
            list.remove(Integer.valueOf(a)); //리스트에서 해당 값을 삭제해야하기 때문에 index가 아닌 객체를 넣어줌
        }

        answer = new int[list.size()];

        for(int i = 0; i < list.size(); i++){
            answer[i] = list.get(i);
        }
        return answer;
    }
}
```

- arr배열을 list로 바꿔준 뒤 delete_list에 있는 숫자들을 list에서 지워준다.
- ArrayList의 remove()함수는 인자로 int와 Object 형태를 받을 수 있다.
  - `ArrayList.remove(int index)` : 해당 인덱스의 값이 삭제 됨
  - `ArrayList.remove(Object o)` : 리스트에서 인자로 받은 객체의 값을 찾아서 첫번째로 나오는 값을 삭제

```toc

```
