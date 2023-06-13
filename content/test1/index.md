---
emoji: 💻
title: 교점에 별 만들기
date: '2023-06-03 00:00:00'
author: 화나
tags: 프로그래머스
categories: 프로그래머스
---

[문제](https://school.programmers.co.kr/learn/courses/30/lessons/87377)

### 문제풀이 흐름

1. 모든 직선 쌍에 대해서 반복을 진행한다. <br>
   1-1. 교점 좌표를 구해서 정수 좌표만 저장하기 <br>
2. 1의 결과에 대해 x, y 좌표의 최댓값, 최솟값 구하기
3. 2의 결과로 2차원 배열의 크기를 결정하고 배열에 별 표시
4. 문자열 배열로 반환

### 풀이

#### 좌표를 나타내는 클래스 작성

```java
private static class Point{
	public final long x, y;

	private Point(long x, long y){
		this.x = x;
		this.y = y;
	}
}
```

- final 사용 이유 ? 데이터를 나타내는 클래스이므로 불변성을 갖게 하기 위해서
- 생성자로 초기화 하도록 함
- long으로 표현해야지 오버플로우가 발생하지 않음

#### 모든 직선 쌍에 대해 반복 진행

```java
for(int i = 0; i < line.length; i++){
	for(int j = i + 1; j < line.length; j++){

	}
}
```

#### 교점 좌표 구하는 메소드

```java
private Point intersection(long a, long b, long e, long c, long d, long f){
	double x = (double) (b * f - e * d) / (a * d - b * c);
	double y = (double) (e * c - a * f) / (a * d - b * c);

	if(x % 1 != 0 || y % 1 != 0) return null

	return new Point((long) x, (long) y);
}
```

#### 교점 좌표 구해서 정수만 리스트에 저장하기

```java
List<Point> points = new ArrayList<Point>();
for(int i = 0; i < line.length; i++){
	for(int j = i + 1; j < line.length; j++){
		Point intersection = intersection(line[i][0], line[i][1], line[i][2], line[j][0], line[j][1], line[j][2]);
		if(intersection != null){
			points.add(intersection);
		}
	}
}
```

#### 저장된 정수 중 최댓값, 최솟값 구하기

```java
private Point getMinPoint(List<Point> points){
	long x = Long.MAX_VALUE;
	long y = Long.MAX_VALUE;

	for(Point p : points){
		if(p.x < x) x = p.x;
		if(p.y < y) y = p.y;
	}

	return new Point(x, y);
}

private Point getMaxPoint(List<Point> points){
	long x = Long.MIN_VALUE;
	long y = Long.MIN_VALUE;

	for(Point p : points){
		if(p.x > x) x = p.x;
		if(p.y > y) y = p.y;
	}

	return new Point(x, y);
}
```

- `Long.MAX_VALUE` : Long 범위 내에서 표현할 수 있는 가장 큰 숫자 값으로 초기화

#### 구한 최솟값, 최댓값을 이용해서 2차원 배열의 크기 결정

```java
Point minVal = getMinPoint(points);
Point maxVal = getMaxPoint(points);

int width = (int) (maxVal.x - minVal.x + 1);
int height = (int) (maxVal.y - minVal.y + 1);

char[][] arr = new char[height][width];

for(char[] row : arr){
	Arrays.fill(row, '.');
}
```

- 좌표를 표현할 수 있는 최소 크기의 2차원 배열을 만들어서 '.' 으로 채워준다.

#### 2차원 배열에 좌표 찍기

```java
for(Point p : points){
	int x = (int) (p.x - minVal.x);
	int y = (int) (maxVal.y - p.y);
	arr[y][x] = '*';
}
```

- 2차원 배열에서 (0,0)은 실제 좌표의 (0,0)이 아니므로 좌표를 변환시켜 주어야 한다.
- [x좌표 - x 좌표 최솟값] 공식으로 좌표를 구한다.
- y좌표는 좌표의 값을 거꾸로 그려줘야 하기 때문에 [y좌표 최댓값 - y좌표] 공식으로 좌표를 구한다.

#### 문자열 배열로 변환 후반환

```java
String[] result = new String[arr.length];
for(int i = 0; i < result.length; i++){
	result[i] = new String(arr[i]);
}

return result;
```

### 최종코드

```java
import java.util.*;

class Solution {

    public String[] solution(int[][] line) {

		//교점 중 정수인 좌표를 담는 리스트
        List<Point> points = new ArrayList<Point>();

		//모든 직선쌍에 대해서 반복
        for(int i = 0; i < line.length; i++){
            for(int j = i+1; j < line.length; j++){
                Point intersection = intersection(line[i][0], line[i][1], line[i][2], line[j][0], line[j][1], line[j][2]);

                if(intersection != null){
                    points.add(intersection);
                }
            }
        }

        Point minVal = getMinPoint(points);
		Point maxVal = getMaxPoint(points);

		int width = (int) (maxVal.x - minVal.x + 1);
		int height = (int) (maxVal.y - minVal.y + 1);

		char[][] arr = new char[height][width];

		for(char[] row : arr){
			Arrays.fill(row, '.');
		}

        for(Point p : points){
			int x = (int) (p.x - minVal.x);
			int y = (int) (maxVal.y - p.y);
			arr[y][x] = '*';
		}

        String[] answer = new String[arr.length];
        for(int i = 0; i < answer.length; i++){
            answer[i] = new String(arr[i]);
        }

        return answer;
    }

    private Point getMinPoint(List<Point> points){
		long x = Long.MAX_VALUE;
		long y = Long.MAX_VALUE;

		for(Point p : points){
			if(p.x < x) x = p.x;
			if(p.y < y) y = p.y;
		}

		return new Point(x, y);
	}

	private Point getMaxPoint(List<Point> points){
		long x = Long.MIN_VALUE;
		long y = Long.MIN_VALUE;

		for(Point p : points){
			if(p.x > x) x = p.x;
			if(p.y > y) y = p.y;
		}

		return new Point(x, y);
	}


	//교점 중 정수를 골라서 담는 메소드
    public Point intersection(long a1, long b1, long c1, long a2, long b2, long c2){
        double x = (double) (b1 * c2 - c1 * b2) / (a1 * b2 - b1 * a2);
        double y = (double) (c1 * a2 - a1 * c2) / (a1 * b2 - b1 * a2);

        if(x % 1 != 0 || y % 1 != 0){
            return null;
        }

        return new Point((long) x, (long) y);
    }

	//좌표를 나타내는 클래스
    private static class Point{
        public final long x, y;

        private Point(long x, long y){
            this.x = x;
            this.y = y;
        }
    }
}
```

```toc

```
