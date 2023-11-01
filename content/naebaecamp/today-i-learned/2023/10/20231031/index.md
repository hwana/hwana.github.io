---
emoji: ✨
title: TIL) parseInt()와 valueOf()의 차이
date: '2023-10-31 00:00:00'
author: 화나
tags: TIL
categories: TIL
---

## 📝 오늘의 내용정리
[알고리즘 스터디 3일차](https://github.com/StudySpringAlgorithm/Study_Algorithm_TeamSpring/blob/main/Kim/day3/day3.md)

코딩테스트 문제를 풀다가 parseInt()와 valueOf() 차이가 궁금해졌다. 겉으로 보기엔 같은 값을 반환하는데 왜 굳이 두 메소드가 따로 존재하는걸까?

### parseInt()와 valueOf()의 차이

```java
String str = "1234";

int a = Integer.parseInt(str);    //1234
Integer b = Integer.valueOf(str); //1234
```

두 메소드 모두 **문자열을 정수로 변환**하고 숫자로 변환할 수 없는 문자열이 입력되었을 때는 NumberFormatException이 발생한다.

두 메소드의 차이점은 **반환값의 타입**이다. 

```java
public static Integer valueOf(String s) throws NumberFormatException {
    return Integer.valueOf(parseInt(s, 10));
}

public static Integer valueOf(String s, int radix) throws NumberFormatException {
    return Integer.valueOf(parseInt(s,radix));
}

public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

첫번째 valueOf()를 보면 인자에 parseInt()가 사용된 것을 확인할 수 있다. 마지막 valueOf()를 보면 반환값으로 새로운 Integer 객체를 생성한다. 

#### 결론

valueOf()는 parseInt()를 호출해서 문자열을 정수값으로 변환시키고 Integer로 박싱해서 값을 반환한다. 두 메소드 모두 반환타입만 다를 뿐 같은 기능이기 때문에 상황에 맞는 메소드를 잘 선택해서 사용하면 된다.

## 🔚 오늘의 마무리
스프링 주차가 시작된 오늘! 새로운 강의가 지급되어서 열심히 들었다. Spring과 Spring Boot의 차이가 나왔다. 스프링부트는 어노테이션 기반으로 내장 톰캣을 가지고 있다는 것! 내장 톰캣을 지원해준다는 점은 개발자를 아주 편리하게 해주는 것 같다. Spring으로 프로젝트 했을 때 환경설정에서 애먹었던 순간이 잠시 떠올라서 잠시 눈을 질끈 감았다😣 부트가 2014년도에 나왔다는데 14년도 이후에 개발하게 돼서 정말 다행이라는 생각이 들었다😋 

요즘 스트림에 익숙해지려고 많이 노력중이다. 나도 상황에 따라 for와 stream을 알맞게 사용하고 싶다. 그래서 오늘도 모던 자바 인 액션을 읽고 동적 파라미터화에 대해 이해했다. 블로그에 적을만큼 완벽하게 이해된건 아니기 때문에 나중에 한번 더 읽어봐야겠다. 내일은 스프링 강의를 마저 듣고 람다 부분을 슬쩍 살펴봐야지!🙄

```toc

```




