---
layout: post
title: "라우팅 알고리즘(Distance vector)"
categories: 네트워크
author: bn-tw2020
---
* content
{:toc}


## 라우팅 알고리즘

* 라우터는 패킷들이 어디로 가야할지 포워딩 테이블을 통해 포워딩해준다.

* 여기서 궁금한 것은 포워딩 테이블을 누가 만들어주는 것이냐는 것이다.

* 포워딩 테이블은 라우팅 알고리즘에 의해 만들어진다.

  





* 라우팅 알고리즘는 크게 2가지로 나뉜다.

  한 가지는 네트워크 상황을 다 알고 있는 경우의 접근 방법인 **link state algorithms** 과

  다른 하나는 내 주변 근접한 이웃(라우터)들만 이야기하며 접근하는 경우는 **distance vector algorithms** 이다.


## distance vector algorithms

* link state 알고리즘은 브로드캐스트해서 직관적으로 모든 정보를 알아서 최단 경로를 찾는다.

* distance vector 알고리즘은 직접적으로 연결되어 있는 이웃과만 메시지를 교환해서 부분적인 정보를 이용해서 최단 경로를 찾는다.

<img width="537" alt="스크린샷 2021-08-28 오전 12 10 00" src="https://user-images.githubusercontent.com/66770613/131149309-648d70b1-b398-4a26-af71-3b693e4cacb8.png">  

<img width="269" alt="스크린샷 2021-08-28 오전 12 10 15" src="https://user-images.githubusercontent.com/66770613/131149346-b4337415-aa3f-4203-8421-9f2c20148f92.png">  

* dx(y)는 x에서 y로 가는 최단 거리이다.

  **x에서 y까지의 최소 경로는 무조건 x의 이웃을 통해 간다. 그리고 나머지는 이웃에서 y까지의 거리**이다.

  나머지는 이웃에서 y까지의 거리는 또 위의 방법으로 재귀적으로 이루어진다.


* 각각의 노드는 distance vector을 관리하게되고 인접한 노드들에게 받으면 최단 경로를 계산할 것이고, 

  자신의 distance vector가 변경되면 주변 노드에게 전달하면서 최신의 상태를 업그레이드 한다.

  **반드시** 자신의 distance vector에 변경이 생기면 주변 이웃 노드에게 전달하게된다.

  바뀌지 않으면 보내지않으면 된다.


<img width="775" alt="스크린샷 2021-08-28 오전 12 17 16" src="https://user-images.githubusercontent.com/66770613/131150281-f3e13c17-1bce-4c9f-90f3-3d5b53c66cd0.png">  
