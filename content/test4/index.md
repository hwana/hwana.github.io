---
emoji: 💻
title: 성격 유형 검사하기
date: '2023-06-21 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/118666)

### 내 최종코드

```java
import java.util.*;
class Solution {
    public String solution(String[] survey, int[] choices) {
        String answer = "";
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        String[] arr = new String[]{"RT", "CF", "JM", "AN"};

        for(int i = 0; i < survey.length; i++){

            //기준점에 되는 4에서 뺀 결과값을 절대값 처리
            int score = Math.abs(4 - choices[i]);
            //choices가 4보다 작다면 survey의 앞글자(비동의), 4보다 크다면 survey의 뒷글자(동의)
            Character c = 4 > choices[i] ? survey[i].charAt(0) : survey[i].charAt(1);


            //map에 이미 문자가 있다면 해당 문자에 점수를 더해주고, 아니면 문자와 점수를 넣어준다.
            if(map.containsKey(c)){
                map.put(c, map.get(c) + score);
            }else{
                map.put(c, score);
            }
        }

        //고정된 배열인 arr을 활용하여 각 지표의 점수를 비교한다.
        for(String s : arr){
            if(map.getOrDefault(s.charAt(0), 0) < map.getOrDefault(s.charAt(1), 0)){
                answer += s.charAt(1);
            }else{
                answer += s.charAt(0);
            }
        }
        return answer;
    }
}
```

#### 다른사람 풀이 참고 후 수정코드

```java
import java.util.*;
class Solution {
    public String solution(String[] survey, int[] choices) {
        String answer = "";
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        Character[][] arr = new Character[][]{{'R', 'T'}, {'C', 'F'}, {'J', 'M'}, {'A', 'N'}};

        //map 초기화
        for(Character[] c : arr){
            map.put(c[0], 0);
            map.put(c[1], 0);
        }

        //해당 문자가 있는지 없는지 판단하는 if문이 빠지게 된다.
        for(int i = 0; i < survey.length; i++){
            int score = Math.abs(4 - choices[i]);
            Character choice = 4 > choices[i] ? survey[i].charAt(0) : survey[i].charAt(1);
            map.put(choice, map.get(choice) + score);
        }

        //getOrDefault메소드를 get메소드로 변경할 수 있다.
        for(Character[] c : arr){
            answer += map.get(c[0]) < map.get(c[1]) ? c[1] : c[0];
        }

        return answer;
    }
}
```

- map을 초기화 해주면 map에 해당 문자가 있는지 없는지 판단하는 코드가 줄어들게 된다.

```toc

```
