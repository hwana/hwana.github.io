---
emoji: ✨
title: TIL) Github Label 커스텀하기
date: '2023-11-03 00:00:00'
author: 화나
tags: TIL
categories: TIL
---

## 📝 오늘의 내용정리

[Github Label 커스텀하기](https://hwana.github.io/util/git-label-setting/)

## 🤪 오늘 만난 오류

Postman으로 테스트를 진행하는데 아래와 같은 오류가 발생했다.

### 첫번째 오류

```
Invalid character found in method name
```

엔드포인트에 습관적으로 https를 입력하여 문제가 발생했다.

http로 변경하니 오류가 사라졌다!

### 두번째 오류

```
[org.springframework.web.HttpMediaTypeNotSupportedException: Content-Type 'text/plain;charset=UTF-8' is not supported]
```

Postman에서 자동 생성해주는 헤더의 Content-Type값이 text/plain으로 설정되어 있어서 발생한 문제였다.

application/json으로 변경해주니 오류가 사라졌다!

## 🔚 오늘의 마무리

오늘은 게시판 CURD API를 만드는 스프링 개인과제가 시작되었다. 사용자도 없는 CRUD라서 시간이 오래걸리지는 않았다. 기본적인 틀은 만들어 놨으니 주말에 추가적인 부분만 조금 더 수정하고 월요일에 제출해야겠다!

```toc

```