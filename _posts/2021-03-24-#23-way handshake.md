---
layout: post
title: "3-way handshake"
categories: 네트워크
author: bn-tw2020
---
* content
{:toc}


## Intro

```
네트워크를 정리하는 글입니다.
```





---

## 2-way handshake

```
2-way로 request와 accept하면 연결되지 않을까?
A와 B에서 연결을 한다고 생각해보자.
A: 우리 이야기할래?
B: 그래.

위와 같이 한다면 A입장은 자신이 말이 B에게 도착하고 답변을 받았으니 연결됨을 알 수 있다.
하지만, B입장에서는 그래라고 답변한 것이 잘 갔는지에 대해 정확히 알수가 없다.
```


## 3-way handshake

```
요청자와 제공자 모두 connection을 확신할 수 있도록 3-way handshake를 사용한다.

⟹ 요청을 보내기 전에 내가 사용할 sequence number(x)를 정하고 상대방에게 알려줘야 한다.
    TCP connection을 요청하는 특수한 세그먼트이다.
    SYNbit = 1, Seq num = x로 설정하여, 보내게 된다.(기본적으로 SYNbit는 0이다.)
⟹ 그럼 받는 쪽에서도 sequence number(y)를 정하고 상대방에게 알려줍니다.
    SYNbit = 1, Seq num = y, ACKbit = 1, ACK num = x + 1로 설정하여 보낸다.
⟹ 마지막으로, 요청을 보낸쪽에서 마지막 응답을 해줍니다.
    ACKbit = 1, ACKnum = y + 1로 설정하여 보냅니다.
```

## Connection Close

```
connection을 닫는다는 것은 보낼 데이터를 다 보냈다는 것이다.  
그럼 할 말이 없고 connection을 끊어주면 된다.
⟹ A가 세그먼트 헤더 필드 중 FIN이라는 필드에 1을 넣어보내주면 된다.
    그럼 상대방(B)은 그 친구(A)가 할 말이 없구나라고 생각하고 ACK를 보내주고, 더 이상 이야기를 안하게 된다.

여기서 주의할 점, A는 할말이 없지만, 상대방(B)는 아직 할말이 남아있을 수도 있다.
그래서 서로 FIN을 보내줘야만 연결이 끊기는것이다.
B도 할말이 없어서 FIN이라는 필드에 1을 넣어보내주면 A는 ACKbit = 1와 ACK num을 보내주게 된다.

하지만, 실제론 B가 FIN을 보내고 ACK를 받아도 끊지말고, 기다리라고 한다.
why?
유실이라는 것을 생각해보자
B가 FIN을 보냈고 A가 ACK를 보내다가 도중에 유실이 된다면,
B측에서는 ACK가 오지않기 때문에 계속 ACK를 기다릴 것이다.
유실됬을 때을 대비해서 일 것이다.
```
