---
layout: post
title: "프림 알고리즘"
categories: 알고리즘
author: bn-tw2020
---
* content
{:toc}

## Intro

```
신장 트리(스패닝 트리)는 그래프의 최소 연결 부분 그래프이다.
최소 신장 트리(MST)는 그래프의 최소 연결 부분의 합이 최소인 신장 트리이다.
즉, 신장 트리이면서 정점과 정점 사이의 경로의 합이 최소인 것이다.
```




---

## 신장 트리란?

<a href="https://bn-tw2020.github.io/2021/02/08/1SpanningTree/">신장트리</a>에 대한 개념을 정리한 글을 참고하시면 좋을 것 같다.

* 그래프에서 MST를 만드는 방법 중에서 많이 알고 알려진 방식으로는 `프림 알고리즘` `크루스칼 알고리즘`이 존재

* 프림 알고리즘: 정점을 추가하면서 트리를 확장하는 방법
   * 시작점을 정하고, 시작점에서 가까운 정점을 선택하면서 MST를 구성하므로 사이클을 이루지 않는다.

* 크루스칼 알고리즘: 간선을 추가하면서 트리를 확장하는 방법
   * 시작점을 정하지 않고 최소 비용의 간선을 찾아가면서 MST를 구성하므로 사이클이 만들어질 수가있다.
   * 신장 트리는 사이클이 이루어지면 안되기에 사이클을 이루는지 확인을 해야한다.
   * 사이클을 확인하는 방법으로는 Union-Find(Disjoin-Set)이 있다.

## Prim Algorithm

* MST이며, 그리디 알고리즘에 속한다.

* 가중치가 있는 연결 무향 그래프의 모든 꼭짓점을 포함하면서 각 변의 비용의 합이 최소가 되는 부분 그래프인 트리

* 한 정점을 기준으로 작은 가중치를 사용해서 모든 정점을 연결하는 트리를 만든다.
   * 최소의 가중치만 사용해서 모든 정점을 연결한다.


```
프림 알고리즘 과정

1. 그래프에서 임의의 하나의 정점을 선택
2. 선택한 정점과 인접하는 정점들 중 최소 비용의 간선이 존재하는 정점을 선택
3. 1,2번의 과정을 반복하여 모든 정점을 선택한다.
```

<img width="389" alt="스크린샷 2021-02-14 오전 1 08 17" src="https://user-images.githubusercontent.com/66770613/107854706-206f9a00-6e61-11eb-930e-102274e8d94c.png">


<img width="368" alt="스크린샷 2021-02-14 오전 1 08 47" src="https://user-images.githubusercontent.com/66770613/107854717-32513d00-6e61-11eb-9a8b-8096e403756d.png">

<img width="368" alt="스크린샷 2021-02-14 오전 1 09 01" src="https://user-images.githubusercontent.com/66770613/107854722-3aa97800-6e61-11eb-9c53-70e9be2dd044.png">

<img width="368" alt="스크린샷 2021-02-14 오전 1 09 56" src="https://user-images.githubusercontent.com/66770613/107854741-5c0a6400-6e61-11eb-9920-aba6d296cc71.png">


<img width="368" alt="스크린샷 2021-02-14 오전 1 10 08" src="https://user-images.githubusercontent.com/66770613/107854745-6298db80-6e61-11eb-81e8-b918a75d2c20.png">

<img width="368" alt="스크린샷 2021-02-14 오전 1 10 36" src="https://user-images.githubusercontent.com/66770613/107854757-73e1e800-6e61-11eb-9da6-5e881dcb1ad1.png">

<img width="368" alt="스크린샷 2021-02-14 오전 1 10 48" src="https://user-images.githubusercontent.com/66770613/107854761-7a705f80-6e61-11eb-975a-60aa9e79e6fe.png">