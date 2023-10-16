---
emoji: 📚
title: Arrays class
date: '2023-10-13 00:00:00'
author: 화나
tags: 자바
categories: 자바
---

> 자주 쓰면서 어떤 메소드가 있는지 정확하게 모르고 썼던 Arrays 클래스!
> 이번에 확실하게 머리에 넣자! 👀 

배열을 다루기 위한 다양한 메소드가 포함되어 있다. 

모두 클래스 메소드 이므로 객체를 생성하지 않고 클래스명으로 접근하여 바로 생성할 수 있다.

### `asList(arrays)`

- 전달받은 배열을 고정 크기의 리스트로 변환하여 반환함

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ArraysAsListExample {
    public static void main(String[] args) {
        // 배열을 List로 변환
        String[] array = {"사과", "바나나", "체리", "딸기"};
        
        // 원래 배열과 연결된 List
        List<String> list1 = Arrays.asList(array);
        
        // ArrayList로 새로운 List 생성
        List<String> list2 = new ArrayList<>(Arrays.asList(array));

        // 배열 수정
        array[0] = "오렌지";
        
        // list1 출력
        System.out.println("list1: " + list1);
        
        // list2 출력
        System.out.println("list2: " + list2);
    }
}
```

> 실행결과
> 
> 
> list1: [오렌지, 바나나, 체리, 딸기] : 원래 배열과 같은 주소를 바라보는 리스트<br>
> list2: [사과, 바나나, 체리, 딸기] : 새로운 객체가 생성되어 별개의 주소값을 가지는 리스트
> 

### `binarySearch(arrays, target)`

- 전달받은 배열에서 특정 값의 위치를 이진 검색 알고리즘을 사용하여 검색한 후 해당 인덱스를 반환하고 찾지 못한 경우 음수 값을 반환함
- 이진 검색 알고리즘을 사용하므로 전달되는 배열이 미리 정렬되어 있어야 제대로 동작함

```java
import java.util.Arrays;

public class BinarySearchExample {
    public static void main(String[] args) {
        int[] sortedArray = {2, 4, 6, 8, 10, 12, 14, 16};

        // 먼저 배열을 정렬해야 합니다.
        Arrays.sort(sortedArray);

        int target = 10;
        int index = Arrays.binarySearch(sortedArray, target);

        if (index >= 0) {
            System.out.println("요소 " + target + "는 배열에서 인덱스 " + index + "에 있습니다.");
        } else {
            System.out.println("요소 " + target + "는 배열에 존재하지 않습니다.");
        }
    }
}
```

> 실행 결과
> 
> 
> 요소 10는 배열에서 인덱스 4에 있습니다.
> 

### `copyOf(arrays, newLength)`

- 전달받은 배열의 특정 길이만큼을 새로운 배열로 복사하여 반환함(깊은복사)
- 새로운 배열의 길이가 원본 배열의 길이보다 크다면 나머지는 배열 요소의 타입에 맞게 기본값으로 채워진다.

```java
import java.util.Arrays;

public class ArraysCopyOfExample {
    public static void main(String[] args) {
        int[] originalArray = {1, 2, 3, 4, 5};

        // 원래 배열의 일부를 복사
        int[] copiedArray1 = Arrays.copyOf(originalArray, 3);
        System.out.println("copiedArray1: " + Arrays.toString(copiedArray1));

        // 원래 배열 전체를 복사
        int[] copiedArray2 = Arrays.copyOf(originalArray, originalArray.length);
        System.out.println("copiedArray2: " + Arrays.toString(copiedArray2));

        // 복사된 배열을 수정해도 원래 배열에 영향을 미치지 않습니다.
        copiedArray1[0] = 10;
        System.out.println("originalArray: " + Arrays.toString(originalArray));
        System.out.println("copiedArray1 after modification: " + Arrays.toString(copiedArray1));
    }
}
```

> 실행결과
> 
> 
> copiedArray1: [1, 2, 3]<br>
> copiedArray2: [1, 2, 3, 4, 5]<br>
> originalArray: [1, 2, 3, 4, 5]<br>
> copiedArray1 after modification: [10, 2, 3]
> 

### `copyOfRange(arrays, from, to)`

- 전달받은 배열의 특정 범위에 해당하는 요소만을 새로운 배열로 복사하여 반환함

```java
import java.util.Arrays;

public class ArraysCopyOfRangeExample {
    public static void main(String[] args) {
        int[] originalArray = {1, 2, 3, 4, 5};

        // 배열의 일부를 복사
        int[] copiedArray = Arrays.copyOfRange(originalArray, 1, 4);

        System.out.println("originalArray: " + Arrays.toString(originalArray));
        System.out.println("copiedArray: " + Arrays.toString(copiedArray));
    }
}
```

> 실행결과
> 
> 
> originalArray: [1, 2, 3, 4, 5]<br>
> copiedArray: [2, 3, 4]
> 

### `equals(arrays1, arrays2)`

- 두 배열이 동일한 요소를 가지고 있는지 요소와 길이를 모두 비교

```java
import java.util.Arrays;

public class ArraysEqualsExample {
    public static void main(String[] args) {
        int[] array1 = {1, 2, 3, 4, 5};
        int[] array2 = {1, 2, 3, 4, 5};
        int[] array3 = {5, 4, 3, 2, 1};

        boolean isEqual1 = Arrays.equals(array1, array2);
        boolean isEqual2 = Arrays.equals(array1, array3);

        System.out.println("array1 and array2 are equal: " + isEqual1);
        System.out.println("array1 and array3 are equal: " + isEqual2);
    }
}
```

> 실행결과
> 
> 
> array1 and array2 are equal: true<br>
> array1 and array3 are equal: false
> 

### `deepEquals(arrays1, arrays2)`

- 다차원 배열의 깊은내용(내부 배열까지) 동일한지 확인하여 boolean 값을 반환함

```java
import java.util.Arrays;

public class ArraysDeepEqualsExample {
    public static void main(String[] args) {
        // 다차원 배열 생성
        int[][] a1 = { {1, 2}, {3, 4} };
        int[][] a2 = { {1, 2}, {3, 4} };
        int[][] a3 = { {1, 2}, {5, 6} };

        // 깊은 내용을 비교
        boolean isEqual1 = Arrays.deepEquals(a1, a2);
        boolean isEqual2 = Arrays.deepEquals(a1, a3);

        System.out.println("a1 and a2 are deep equals: " + isEqual1);
        System.out.println("a1 and a3 are deep equals: " + isEqual2);
    }
}
```

> 실행결과
> 
> 
> a1 and a2 are deep equals: true<br>
> a1 and a3 are deep equals: false
> 

> `equals` 와 `deepEquals` 의 차이점
> |  | equals  | deepEquals  |
> | --- | --- | --- |
> | 배열의 종류 | 1차원 배열 | 다차원 배열 |
> | 비교 방식 | 내용과 길이 모두 비교 | 내용과 길이와 구조를 모두 비교 |

### `hashCode(arrays)`

- 배열의 내용의 길이만 비교하여 단순한 해시코드 생성

```java
import java.util.Arrays;

public class ArraysHashCodeExample {
    public static void main(String[] args) {
        int[] array1 = {1, 2, 3, 4, 5};
        int[] array2 = {1, 2, 3, 4, 5};
        int[] array3 = {5, 4, 3, 2, 1};

        int hashCode1 = Arrays.hashCode(array1);
        int hashCode2 = Arrays.hashCode(array2);
        int hashCode3 = Arrays.hashCode(array3);

        System.out.println("HashCode of array1: " + hashCode1);
        System.out.println("HashCode of array2: " + hashCode2);
        System.out.println("HashCode of array3: " + hashCode3);
    }
}
```

> 실행결과
> 
> 
> HashCode of array1: 538279632<br>
> HashCode of array2: 538279632<br>
> HashCode of array3: 28229436
> 

### `deepHashCode(arrays)`

- 다차원 배열의 깊은내용(내부 배열까지) 에 대한 해시코드 생성
- 배열의 내용과 길이 구조까지 모두 비교하여 해시코드 생성

```java
import java.util.Arrays;

import java.util.Arrays;

public class ArraysDeepHashCodeExample {
    public static void main(String[] args) {
        int[][] array1 = {{1, 2, 3}, {4, 5, 6}};
        int[][] array2 = {{1, 2}, {3, 4, 5, 6}};
        int[][] array3 = {{1, 2, 3}, {4, 5, 6}};

        int deepHashCode1 = Arrays.deepHashCode(array1);
        int deepHashCode2 = Arrays.deepHashCode(array2);
        int deepHashCode3 = Arrays.deepHashCode(array3);

        System.out.println("DeepHashCode of array1: " + deepHashCode1);
        System.out.println("DeepHashCode of array2: " + deepHashCode2);
        System.out.println("DeepHashCode of array3: " + deepHashCode3);
    }
}
```

> 실행결과
> 
> 
> DeepHashCode of array1: -1920740469<br>
> DeepHashCode of array2: 2002671652<br>
> DeepHashCode of array3: -1920740469
> 

### `toString(arrays)`

- 배열의 내용을 문자열로 표현
- 다차원 배열은 내부의 참조까지만 표현, 요소는 출력하지 않는다.

```java
import java.util.Arrays;

public class ArraysToStringExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};

        String arrayAsString = Arrays.toString(numbers);

        System.out.println("Array as String: " + arrayAsString);
    }
}
```

> 실행결과
> 
> 
> Array as String: [1, 2, 3, 4, 5]
> 

### `deepToString(arrays)`

- 다차원 배열의 깊은내용(내부 배열까지) 출력

```java
import java.util.Arrays;

public class ArraysDeepToStringExample {
    public static void main(String[] args) {
        // 다차원 배열 생성
        int[][] array = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9} };

        // 다차원 배열 출력
        String arrayString = Arrays.deepToString(array);

        System.out.println("Array: " + arrayString);
    }
}
```

> 실행결과
> 
> 
> Array: [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
> 

### `fill(arrays, value)`

- 배열의 모든 요소를 지정된 값으로 채움

```java
import java.util.Arrays;

public class ArraysFillExample {
    public static void main(String[] args) {
        int[] array = new int[5];

        // 배열을 0으로 채움
        Arrays.fill(array, 0);

        // 배열 출력
        System.out.println("Array after filling with 0: " + Arrays.toString(array));

        // 배열을 1로 채움
        Arrays.fill(array, 1);

        // 배열 출력
        System.out.println("Array after filling with 1: " + Arrays.toString(array));
    }
}
```

> 실행결과
> 
> 
> Array after filling with 0: [0, 0, 0, 0, 0]<br>
> Array after filling with 1: [1, 1, 1, 1, 1]
> 

### `parallelPrefix(arrays, operator)`

- JAVA8 이후로 도입된 것으로, 배열의 요소에 대해 병렬 연산을 수행하여 누적 값을 구함

```java
import java.util.Arrays;

public class ArraysParallelPrefixExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8};

        Arrays.parallelPrefix(numbers, (x, y) -> x + y);

        System.out.println("Cumulative Sum of Numbers: " + Arrays.toString(numbers));
    }
}
```

> 실행결과
> 
> 
> Cumulative Sum of Numbers: [1, 3, 6, 10, 15, 21, 28, 36]
> 

### `setAll(arrays, generator)`

- 각 요소를 인덱스 기반으로 설정

```java
import java.util.Arrays;

public class ArraysSetAllExample {
    public static void main(String[] args) {
        int[] numbers = new int[10];

        Arrays.setAll(numbers, index -> index);

        System.out.println("Array with Values Set by Index: " + Arrays.toString(numbers));
    }
}
```

> 실행결과
> 
> 
> Array with Values Set by Index: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
> 

### `parallelSetAll(arrays, generator)`

- 각 요소의 인덱스를 입력으로 받아 해당 인덱스에 대한 값을 계산하고, 배열의 요소를 병렬로 설정함

```java
import java.util.Arrays;

public class ArraysParallelSetAllExample {
    public static void main(String[] args) {
        int[] numbers = new int[10];

        Arrays.parallelSetAll(numbers, index -> index);

        System.out.println("Array with Values Set by Index: " + Arrays.toString(numbers));
    }
}
```

> 실행결과
> 
> 
> Array with Values Set by Index: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
> 

### `sort(arrays)`

- 배열을 정렬하는데 사용

```java
import java.util.Arrays;

public class ArraysSortExample {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 9, 1, 5, 6};

        Arrays.sort(numbers);

        System.out.println("Sorted Array: " + Arrays.toString(numbers));
    }
}
```

> 실행결과
> 
> 
> Sorted Array: [1, 2, 5, 5, 6, 9]
> 

### `parallelSort(arrays)`

- JAVA8부터 도입, 배열을 병렬로 정렬하는데 사용

```java
import java.util.Arrays;

public class ArraysParallelSortExample {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 9, 1, 5, 6};

        Arrays.parallelSort(numbers);

        System.out.println("Sorted Array: " + Arrays.toString(numbers));
    }
}
```

> 실행결과
> 
> 
> Sorted Array: [1, 2, 5, 5, 6, 9]
> 

### `stream(arrays)`

- 배열을 스트림으로 변환하는데 사용
- 배열의 요소를 스트림으로 처리하고 스트림 API를 활용하여 데이터를 처리할 수 있음

```java
import java.util.Arrays;
import java.util.stream.IntStream;

public class ArraysStreamExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};

        IntStream stream = Arrays.stream(numbers);

        stream.forEach(System.out::println);
    }
}
```

> 실행결과
> 
> 
> 1<br>
> 2<br>
> 3<br>
> 4<br>
> 5<br>
>


```toc

```