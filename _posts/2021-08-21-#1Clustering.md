---
layout: post
title: "클러스터링(Clustering)"
categories: DB
author: bn-tw2020
---
* content
{:toc}


## 클러스터링(Clustering)

* DB 분산 기법 중 하나로 DB서버를 여러 개 두어 서버 한 대가 죽었을 때 대비하는 기법입니다.





## DB 서버의 다중화

* 간단히 병렬화해서 대수를 증가시키는 웹서버나 애플리케이션 서버와 비교하면

  다중화에 대해 고민해야 할 부분이 많습니다.

  그 이유는 DB서버가 데이터를 보존하는 `영속 계층`이기 떄문입니다.

* 데이터베이스는 웹 서버나 애플리케이션 서버와 다르게 데이터를 장기간 보존하는 매체가 필요하다.

* 웹 서버나 애플리케이션 서버는 데이터를 일시적으로 처리하지만 데이터베이스는 대량의 데이터를

  영구적으로 보존해야 하고 그에 따른 성능도 요구되기 때문에 데이터를 보존하는 매체에 필요한 요건이 높습니다.

* 일반적으로 서버 내주의 로컬 저장소나 메모리로는 이런 요건을 충족시키지 못하기 때문에 전용의 외부 저장소를 사용합니다.

  그래서, DB서버의 아키텍처는 저장소와 묶어서 생각해야 합니다.


## 다중화 방법

* 기본적으로 DB서버만을 다중화하고 저장소는 하나만 두는 구성을 하게 되면

  데이터가 보존되는 저장소가 1개라서 정합성을 신경 쓸 필요가 없습니다.

* DB서버가 동시에 동작하는 것을 허락할지, 말지에 대해 Active-Actvie와 Active-Standby로 있습니다.

> Active - Active Clustering

<img width="296" alt="스크린샷 2021-08-21 오전 1 01 46" src="https://user-images.githubusercontent.com/66770613/130261329-dfeb7a0b-d3bc-4143-af12-45ce18d70458.png">  

* 클러스터를 구성하는 컴포넌트를 동시에 가동합니다.

* 장점으로 시스템 다운 시간이 짧습니다.

  복수의 DB서버가 동시에 동작하고 있어서 한 대가 다운되어 동작 불가되도 남은 서버가 처리합니다.

  시스템이 정지하는 것을 방지할 수 있습니다. CPU와 메모리 이용률이 증가

  이것은 웹 서버나 애플리케이션 서버의 클러스터링으로 얻을 수 있는 장점과 같다.

* 성능이 좋습니다.

  DB서버 대수가 증가하면 동시에 가동하는 CPU나 메모리도 증가하기 때문에 성능향상이 될 수 있다.

  저장소가 병목되기 때문에 생각한 만큼 성능이 향상되지 않을 수도 있습니다.

  여러 서버가 동시에 운영되서 비용이 많이 발생합니다.


> Active - Standby Clustering

<img width="285" alt="스크린샷 2021-08-21 오전 1 02 24" src="https://user-images.githubusercontent.com/66770613/130261378-24b71f3e-e251-4fb1-8baf-f3a0871600ce.png">  

* 클러스터를 구성하는 컴포넌트 중 실제로 가동하는 것은 Active 남은 것은 대기(Standby)하고 있습니다.

* Cold-Standby : 평소 StandbyDB가 작동하지 않다가 ActiveDB가 다운된 시점에 동작하는 구성

  Hot-Standby : 평소에도 StandbyDB가 작동하는 구성

* Active-Active 클러스터링에 비해 적은 비용 발생

* 서버가 다운 되었을 때 상태 전환 시 시간이 발생

## Question

* Active-Standby서버에서 장애가 있어났을 시 StandbyDB서버는 어떻게 ActiveDB서버에 장애가 일어난 것을 알 수 있을까?

  => StandbyDB서버는 일정 간격으로 ActiveDB에 이상이 없는지 조사하기 위한 통신을 하고 있으며

  이 통신은 `Heartbeat`라고 합니다. ActiveDB에 장애가 발생하면 이 신호가 끊기에 
     
  Standby는 Active가 죽었음을 알 수 있다.


  ## Reference

  ![](https://dheldh77.tistory.com/entry/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%A7%81Clustering?category=805412)