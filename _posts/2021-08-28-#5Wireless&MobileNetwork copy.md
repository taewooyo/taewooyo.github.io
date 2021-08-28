---
layout: post
title: "무선 네트워크 Mobility"
categories: 네트워크
author: bn-tw2020
---
* content
{:toc}


## 무선 네트워크 

* 대학교 강의실A에서 a라는 AP에 접속해서 2시간 짜리 영화를 보기 시작했다. 영화를 보면서 할일이 생겨서

  강의실B로 이동했다. 이동하는 와중에 AP가 바뀔 것이다. AP가 바뀌는 상황에서 Connection이 유지될까? 끊길까?

* Connection은 전송계층에서의 TCP프로토콜의 end-to-end의 연결이다. 소켓과 소켓이 연결한 유일한 연결이다.

  TCP Connection은 src IP address, port와 dest address, port로 인덱싱한다.

  전세계에서 유일한 연결이다.
 
* 여기서 src IP address, port와 dest address, port 중 하나만 변경되면 끝나게 된다.





* 한 자리에서 계속해서 영화를 본다면 영화 정보를 가져오는 dest IP address, port는 변화하지 않는다.

  물론, 내가 자리를 유지하고 보기 때문에 src IP address, port도 변하지 않기 때문에 Connection이 유지된다.

* 이동하는데 Connection이 끊기냐 안끊기냐는 내 IP address, port가 변화하는지 안하는지를 보면된다.

  위의 질문에는 한 네트워크 내에 있다면 변화하지 않기 때문에 끊기지 않는다.

* 즉, 확인해야할 것은 원래 오던 경로가 아닌 다른 경로로만 오게되는 것이다.

  다른 경로로 오는 것은 스위치에서 self learning을 통해 스스로 업데이트하게 된다.

* 위의 설명은 같은 네트워크에 있다는 가정에서 이루어지는 현상이다.


> Q 휴대폰으로 집에서부터 PC방을 가는데 유튜브를 보면서 가면 전혀 끊기지 않고 영상을 볼 수 있다 왜그럴까?

* 위에서 말한대로 Connection이 유지된다는 의미이다.

  위에서 대학교 내 와이파이를 사용할 떄 끊기지 않는다고 했다. 왜냐하면 AP는 바껴도 물리적으로 대학교 네트워크에서 이루어지기 때문이다.

* 그럼 위의 질문의 경우에는 SKT라고 하면 SKT 범위는 전국이다. 결국 스위치 같은 것들끼리 연결되어 있어서 가능하다.



