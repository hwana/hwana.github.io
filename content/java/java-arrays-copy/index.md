---
emoji: 📚
title: 배열의 얕은복사와 깊은복사
date: '2023-10-13 00:00:00'
author: 화나
tags: 자바
categories: 자바
---

배열은 한번 생성하면 길이를 변경할 수 없다.

배열의 길이를 변경하기 위해서는 새로운 배열을 생성하고 데이터를 복사해야 한다.

배열을 복사하는 방법에는 **얕은복사**와 **깊은복사**가 있다.

1. 얕은복사
    - 얕은복사는 배열의 복사본이 원본 배열과 동일한 객체를 참조한다.
    - 배열의 주소만 복사되고, 내부 요소들은 복사되지 않는다.
    - 요소를 변경하게 되면 두 배열의 모든 값이 변경된다.
    
      ```java
        int[] originalArray = {1, 2, 3, 4, 5};
        int[] shallowCopy = originalArray; // 얕은 복사
        
        shallowCopy[0] = 99;
        // 출력: [99, 2, 3, 4, 5]
        System.out.println(Arrays.toString(originalArray)); 
      ```
    
2. 깊은복사
    - 깊은복사는 원본 배열과 복사본이 서로 다른 객체를 참조한다.
    - 하나의 배열을 변경해도 다른 배열에는 영향을 주지 않는다.
    
      ```java
        //for문을 활용하여 깊은복사 하는 방법
        int[] originalArray = {1, 2, 3, 4, 5};
        int[] deepCopy = new int[originalArray.length];
        
        for (int i = 0; i < originalArray.length; i++) {
            deepCopy[i] = originalArray[i];
        }
        
        deepCopy[0] = 99;
        //원본이 변경되지 않는다.
        // 출력: [1, 2, 3, 4, 5]
        System.out.println(Arrays.toString(originalArray)); 
        
        //Arrays.copyOf 메소드 활용하여 깊은복사 하는 방법
        int[] originalArray = {1, 2, 3, 4, 5};
        int[] deepCopy = Arrays.copyOf(originalArray, originalArray.length);
        
        deepCopy[0] = 99;
        //원본이 변경되지 않는다.
        // 출력: [1, 2, 3, 4, 5]
        System.out.println(Arrays.toString(originalArray)); 
      ```


