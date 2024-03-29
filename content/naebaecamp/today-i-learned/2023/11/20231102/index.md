---
emoji: ✨
title: TIL) 이진탐색(Binary Search)
date: '2023-11-02 00:00:00'
author: 화나
tags: TIL
categories: TIL
---

## 📝 오늘의 내용정리

### 이진탐색(Binary Search)

찾고자 하는 정답이 포함된 범위 중 가운데 요소와 정답을 비교하여 절반의 범위를 제외하면서 요소를 찾아가는 방법
이진탐색을 사용하려면 배일이나 리스트가 정렬되어 있어야 한다.

### 분할정복

탐색 공간을 특정 기준에 따라 나누고, 나누어진 탐색공간에서 탐색을 이어나가는 것
이진 탐색은 업다운 게임처럼 남아있는 범위의 가운데 숫자를 부르고 범위를 계속해서 반으로 나누어 간다.
이진 탐색은 한쪽 범위를 아예 제외하기 때문에 분할 정복을 이용한 효율 높은 탐색 알고리즘이다.

### 범위찾기

```java
int start = 0;
int end = arr.length;
```

배열의 인덱스 범위인 0부터 arr.length-1이 탐색 범위가 된다.

### 검사 진행하기

범위 내의 속한 원소 개수는 end - start이다.

탐색 공간이 남아 있지 않을 때 까지 탐색하려면 end - start가 양수일 때 탐색을 계속 반복해야 한다.

end - start > 0 은 end > start 이므로 조건으로 설정해준다.

```java
while(end > start){

}
```

### 중간 값 비교하기

```java
int mid = (start + end) / 2;
int value = arr[mid];

if(value == target){
		return mid;
}else if(value > target){
		end = mid; // down
}else{
		start = mid + 1;//up
}
```

중간 값과 우리가 찾는 값의 대소를 비교하고, 비교한 결과에 따라 범위를 조절해주어야 한다.

1. 중간 값이 우리가 찾는 값과 같다면
    - 정답을 찾았으므로 해당 값의 인덱스인 mid 반환
2. value가 더 크다면
    - 정답은 더 작은 범위에 있으므로 작은 범위에서 탐색을 이어가야 함
3. value가 더 작다면
    - 정답은 더 큰 범위에 있으므로 큰 범위에서 탐색을 이어가야 함

### 자바의 이진탐색 메소드

배열 : `Arrays.binarySearch()`

리스트 : `Collections.binarySearch()`

이 두 메소드는 배열이나 리스트에 원소가 있다면 해당 원소의 인덱스를 반환한다.

배열이나 리스트에 원소가 없다면 음수가 반환된다.

음수 반환값에을 양수로 변환하고 1을 빼면 찾는 원소가 들어갈 위치가 된다.

```java
int[] array = new int[]{1, 4, 6, 7, 8, 10, 13, 17};
List<Integer> list = List.of(1, 4, 6, 7, 8, 10, 13, 17);

int arrayIndex = Arrays.binarySearch(array, 11);    // -7;
int listIndex = Collections.binarySearch(list, 11); // -7;
```

위 예시로 보면 -7이 반환되었기 때문에 7로 변환한뒤 1을 빼면 6번 위치에 11이 들어가야 함을 알 수 있다.

## 🔚오늘의 마무리
오늘은 내일 알고리즘 스터디원들과 함께 이야기 나눌 이진탐색에 대해서 간단하게 정리해봤다. 

스터디 4일차에 느낀점 : 혼자서 문제를 풀고 끝내는 것보다 여러 사람들과 이야기를 나누는게 내 성장에 도움이 된다는 것!

내일 이진탐색에 대해서 이야기를 나눌때도 내가 정리한 것 외에 다른 사람이 바라본 이진 탐색에 대해 들을 수 있을거 같아서 기대가 된다🤩 캠프가 끝날 때 까지 꾸준히 해서 알고리즘 마스터가 되어야지!

```toc

```