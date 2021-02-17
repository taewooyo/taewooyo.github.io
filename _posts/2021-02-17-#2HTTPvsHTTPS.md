---
layout: post
title: "HTTP vs HTTPS"
categories: 네트워크
author: bn-tw2020
---
* content
{:toc}

## Intro

```
HTTP와 HTTPS의 차이점을 정리하는 글입니다.
```




---

## HTTP

```
HTTP는 Hyper Text Transfer Protocol의 약자이며, 하이퍼텍스트 전송 규칙이다.

하이퍼텍스트는 html과 같이 정보를 효과적으로 전달하기 위한 문서를 칭한다.
즉, HTTP는 html과 같은 문서를 전송할 때 따라야 하는 규칙들을 정의해놓은 것이다.

HTTP는 TCP/IP 모델의 응용 계층(Layer4)에 속하는 프로토콜이며, 주로 서버와 클라이언트 사이에서 html문서를 교환할 때 사용한다.
문서를 전송할 때는 TCP와 UDP를 통해 전송하며, 80번 포트를 사용한다.

HTTP는 클라이언트에서 서버로 요청(Request)을 보낼 때 따라야 할 형식
서버에서 클라이언트로 응답(Response)을 보낼 때 따라야 할 형식을 정의한다.
```

<img width="814" alt="스크린샷 2021-02-17 오후 11 11 32" src="https://user-images.githubusercontent.com/66770613/108216144-7ac86d80-7175-11eb-9ae9-a8062134ec21.png">

```
HTTP 요청과 응답은 기본적으로 다음과 같은 공통적인 구조를 갖는다.
첫번째 줄에는 요청 방식(요청)과, HTTP 버전, 응답 코드(응답) 등의 기본적인 정보,
두번째 줄부터는 본문에 대한 설명을 담는 header, 그리고 1개의 empty line 뒤에 본문인 body 정보를 담는다.
```