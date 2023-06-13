---
emoji: 💻
title: 삼각 달팽이
date: '2023-06-13 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/68645)

### 문제풀이 흐름

1. n\*n 2차원 배열 선언하기
2. 숫자를 채울 현재의 위치를 0,0 으로 설정
3. 방향에 따라 이동할 수 없을때까지 반복하면서 숫자 채우기 <br>
   3-1. 아래로 이동 <br>
   3-2. 오른쪽으로 이동 <br>
   3-3. 왼쪽 위로 이동 <br>
4. 채워진 숫자를 차례대로 1차원 배열에 옮겨서 반환하기

### 풀이

> dx, dy로 방향을 정하는 방법 <br>
> dx, dy는 각각 x의 변화량, y의 변화량을 뜻한다. <br>
> 변화량이란 특정 방향으로 이동할 때 해당 좌표 값이 어떻게 변화하는지를 의미한다. <br>
> dx, dy는 상하좌우 네 방향에 따라서 아래와 같은 값을 가진다. <br>
> ||상|하|좌|우|
> |-||-||-||-||-|
> |dx|0|0|-1|1|
> |dy|-1|1|0|0|

#### n\*n 2차원 배열 선언하기

```java
int[][] arr = new int[n][n];
```

#### 숫자를 채울 현재 위치를 0,0 으로 설정

```java
//위치변수
int x = 0;
int y = 0;
```

#### 방향에 따라 이동할 수 없을 때까지 반복하면서 숫자 채우기

```java
int v = 1; //배열에 담을 수
int d = 0; //방향 변수

while(true){
	arr[y][x] = v++; //2차원 배열에 숫자 집어넣기

	//배열에 숫자를 넣은 후 방향 이동 시켜준다.
	int nx = x + dx[d];
	int ny = y + dy[d];

	//이동시켜준 방향이 배열의 길이와 같거나(아래, 오른쪽 방향), -1이거나(왼쪽 위 방향), 이미 숫자가 채워져 있을때 방향 전환을 해준다.
	if(nx == n || ny == n || nx == -1 || ny == -1 || arr[ny][nx] != 0){
		d = (d + 1) % 3;

		//전환된 방향의 위치를 넣는다.
		nx = x + dx[d];
		ny = y + dy[d];

		//전환된 방향의 위치도 아래 조건을 충족한다면 2차원 배열에 이미 숫자가 가득 차 있는 것이기 때문에 반복문 탈출
		if(nx == n || ny == n || nx == -1 || ny == -1 || arr[ny][nx] != 0) break;
	}

	//전환된 방향의 최종 위치를 위치 변수에 넣어준다.
	x = nx;
	y = ny;
}
```

#### 채워진 숫자를 차례대로 1차원 배열에 옮겨서 반환하기

```java
int[] answer = new int[v-1];
int index = 0;
for(int i = 0; i < n; i++){
	for(int j = 0; j <= i; j++){
		answer[index++] = arr[i][j];
	}
}
```

- 변수 v에는 채워넣은 숫자 마지막 + 1 숫자가 들어있으므로 v-1이 채워 넣은 숫자의 개수가 된다.

### 최종코드

```java
class Solution {
    //아래, 오른쪽, 왼쪽 위
    private static final int[] dx = {0, 1, -1};
    private static final int[] dy = {1, 0, -1};

    public int[] solution(int n) {
        int[][] arr = new int[n][n];
        int x = 0;
        int y = 0;
        int v = 1; //배열에 담을 수
        int d = 0; //방향 변수

        while(true){
            arr[y][x] = v++;

            int nx = x + dx[d];
            int ny = y + dy[d];

            if(nx == n || ny == n || nx == -1 || ny == -1 || arr[ny][nx] != 0){
                d = (d + 1) % 3;
                nx = x + dx[d];
                ny = y + dy[d];
                if(nx == n || ny == n || nx == -1 || ny == -1 || arr[ny][nx] != 0) break;
            }

            x = nx;
            y = ny;
        }

        int[] answer = new int[v-1];
        int index = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j <= i; j++){
                answer[index++] = arr[i][j];
            }
        }

        return answer;
    }
}
```

```toc

```
