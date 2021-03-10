---
layout: post
title: "어플리케이션 계층 및 종류(2)"
categories: 네트워크
author: bn-tw2020
---
* content
{:toc}


## Intro

```
네트워크 계층을 정리하는 글입니다.
```





---

## Web and HTTP

* 웹이라는 것은 객체이다.
   * 객체는 HTML file, JPEG image, JAva applet, audoi file ...가 있다.

```
HTTP : Hypertext transfer protocol

말 그대로 하이퍼텍스트를 전송하는 프로토콜이다.
하이퍼텍스트는 텍스트인데 링크, 그림 등을 포함한 것이라고 생각하면 된다.

HTTP는 request, response 2가지만 존재한다.

어플리케이션이기 때문에 TCP 혹은 UDP를 사용하는데 HTTP는 TCP를 사용한다.
HTTP 프로토콜은 포트를 80번를 이용하게 된다.

HTTP는 stateless이다.
   즉, 상대방에 대해서 아무것도 기억하지 않는다는 의미이다.
   request, response을 하게 되면, 상태를 끊어버리게 된다.
```


HTTP는 TCP를 사용한다고 했는데 2가지의 종류가 존재한다.
1. non-persistent HTTP
   * TCP Connection을 계속해서 새로 만드는 것이다.

2. peristent HTTP
   * TCP Connection를 만들어 놓은 것을 계속 사용한다.

```
Example) Non-persistent HTTP
www.someSchool.edu/someDepartment/home.index를 요청한다고 하자.
(여기에는 10개의 jpeg images, text 객체들이 존재한다.)

우선, HTTP request를 보내기 위해선 TCP Connection이 존재해야 한다.

TCP Connection을 위해서 TCP 메시지가 갖다가 온다. 그 후에 HTTP request를 보내게 된다.
그러면 home.index가 넘어온다. 하지만 home.index에는 10개의 하이퍼텍스트들이 존재한다.
10개의 request를 더 요청해야되는데 Non-persistent HTTP이기 때문에 TCP Connection을 끊고
다시 TCP Connection을 생성한다.

Persistent HTTP는 그러면 TCP Connection을 끊지 않고 진행한다는 것을 이해할 수 있다.
```

![HTTP 메시지 request, response](https://bn-tw2020.github.io/2021/02/17/1_HTTP/)  


## HTTP Stateless

HTTP는 Stateless라고 했었다.  
아마존에서 로그인을 하지도 않았는데 아마존은 내가 혹할만한 것들을 화면에 보여준다.  
웹 서버에서는 쿠키라는 것을 사용해서 사용자를 기억하기도 한다.  

```
client와 server, database가 있다고 생각해보자.

1. 아마존 서버에 방문하려고 한다.
2. 아마존 서버에 방문하기 전에 http request를 보내기 전에 아마존 서버로부터 쿠키를 받은 적이 있는 지확인을 한다.
3. 쿠키가 없다고 가정하자.
4. request을 보낸다.(네트워크 책 한권을 산다)를 쿠키 없이 보낸다.
5. 아마존 서버는 쿠키가 없는 것을 확인하고 쿠키를 만든다.(쿠키는 1678이라고 가정하자)
6. 데이터베이스에서 1678은 책을 구매한다고 기억하며,
7. 사용자에게 생성했던 쿠키1678을 response을 해준다.
8. 사용자는 이제 request를 보낼때마다 쿠키 1678을 보낸다.
9. 이제 일주일 뒤에 아마존에 접근하다고 해보자.
10. 일주일동안 아마존은 사용자에 대한 분석을 계속 했었을 것이다.
11. 사용자는 존재했던 쿠키를 보내주면, 아마존 서버는 아 저번에 그 사람구나!하고 그 사용자가 좋아할 것들을 포함해서 response를 보낸다.

이렇게 쿠키를 통해서 stateless를 연결된 것같은 트릭을 만들 수 있다. 그래서 우리는 쓸데 없는 물건을 살 수도 있게 된다.
```

## Web caches

* proxy server

```
서버로부터 가져온 객체을 자신이 보관을 하다가 다른 클라이언트가 요청을 하게 되면, 자신이 가지고 있으면 자기가 대신 제공을 해준다.

장점?
   1. 사용자의 관점에서 빠르다.
   2. 서버의 관점에서 request가 덜 오기 때문에 과부하 방지가 된다.
   3. 네트워크 소유자에게도 만족감이 생긴다.

단점?
   1. 원본 서버에서 데이터가 변경됬다면 캐시는 틀린 정보를 갖게 있게 된다.
      * 해결책으로 Conditional GET을 이용한다.
      * http는 request 시 If-modified-since: <data>을 통해 서버는 <data>와 수정된 날짜와 비교하고 수정된 경우에만 실제 데이터를 보내준다.

Conditional GET은 프록시 서버가 주기적으로 사용하게 된다.
```

