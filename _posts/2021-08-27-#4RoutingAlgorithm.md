---
layout: post
title: "라우팅 알고리즘(Link State)"
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


## Link State algorithms

* 네트워크 정보를 다 알고 있어야 한다.

  서로 연결이 직접적으로 연결되어 있지 않는데 다른 링크 정보들까지 알기 위해서는 

  자신의 링크 정보를 전체 네트워크에 브로드캐스트를 해야한다.

* 이름이 Link State인 이유는 각 노드들이 자신의 링크 정보들을 전체 네트워크에 브로트캐스트해서 알려준다고 해서이다.

* 여기서는 **다익스트라 알고리즘** 이 사용된다.

  목표는 라우터에서 포워딩 테이블을 채우는 것이다. 

<img width="528" alt="스크린샷 2021-08-27 오후 11 41 52" src="https://user-images.githubusercontent.com/66770613/131145165-c4e3da6e-7698-4b73-a140-95c4f4c055d2.png">
  

```
N' => 확실하게 최단 경로가 결정된 노드이다.
c(x, y) => x에서 y로 가는 거리, 비용이다.
d(v) => 시작지로부터 v까지의 거리이다.
```

<img width="469" alt="스크린샷 2021-08-27 오후 11 44 28" src="https://user-images.githubusercontent.com/66770613/131145581-2739b996-cb9a-412e-9fd4-524f115f1c43.png">  

<img width="418" alt="스크린샷 2021-08-27 오후 11 45 11" src="https://user-images.githubusercontent.com/66770613/131145666-19abd087-83ee-4c90-8f95-063ef2a70cd0.png">  

* 그러면, 모든 네트워크가 브로드캐스트 해서 모든 정보를 줘야하나? 그러면 과부하 걸리지 않을까?

  가능할까? 한국에도 라우터가 있고 미국에도 라우터가 있을 것인데 거기까지 브로드캐스트를 보낼 수가 없다

  그래서, 실제로 브로드캐스트의 범위는 한국대학교라면 한국대학교 하나의 네트워크에서 이루어질 것이고, KT면 KT네트워크에서, SKT면 SKT네트워크에서 한다.

  네트워크 주체가 같은 곳끼리 한다. 내부 네트워크에서 주로 한다고 생각하면 될 것 같다.