---
layout: post
title: "레플리케이션(Replication)"
categories: DB
author: bn-tw2020
---
* content
{:toc}


## 레플리케이션(Replication)

* DB Storage가 손상 혹은 손실되었을 때의 대안으로 Storage를 복제하는 기법입니다.

* 클러스터링 구성에서는 DB서버를 다중화할 수 있어도 저장소 부분은 다중화 할 수 없어서 데이터를

  다중화하지 않는 공통적인 단점이 있습니다. 저장소가 부서질 경우에 데이터를 잃게 됩니다.

* 리플리케이션은 데이터베이스 서버와 저장소가 동시에 사용 불능일 때, 서비스를 계속 할 수 있는

  가용성 높은 아키텍처입니다.





## 레플리케이션의 방법

> 단순 백업

<img width="331" alt="스크린샷 2021-08-21 오전 12 37 12" src="https://user-images.githubusercontent.com/66770613/130258019-beb2857c-3cdb-4a60-adca-7aea49ea9342.png">  

* MasterDB에 CRUD Operation이 수행될 때마다 SlaveDB에 바이너리 로그를 전달하여 데이터를 동기화 하는 방법입니다.

* MasterDB가 손실되었을 때, SlaveDB를 MasterDB로 승격시켜 백업 기능을 수행할 수 있습니다.


> 부하 분산

<img width="342" alt="스크린샷 2021-08-21 오전 12 37 27" src="https://user-images.githubusercontent.com/66770613/130258053-09de95c2-1e5e-49ee-9e6c-24061970efbd.png">  

* 기존 단순 백업 모델에서 SELECT 작업을 SlaveDB에서 수행함으로써 MasterDB의 부하를 분산합니다.

