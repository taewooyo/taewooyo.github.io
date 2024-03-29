---
layout: post
title: "[QnA] 논리적 데이터 모델링(관계 데이터 모델링)"
categories: DB
author: bn-tw2020
---
* content
{:toc}


## Intro

```
관계 데이터 모델의 개념, 관계 데이터 모델의 제약을 확인합니다.
```





---

* 관계 데이터 모델이 무엇인가요?

```
개념적 구조를 논리적 구조로 표현하는 논리적 데이터 모델입니다.
하나의 개체에 대한 데이터를 하나의 릴레이션에 저장하게 됩니다.
```

* 관계 데이터 모델의 용어가 무엇이 있나요?

```
릴레이션 : 하나의 개체에 관한 데이터를 2차원 테이블 구조로 저장한 것
         파일 관리 시스템 관점에서 파일에 대응
속성 : 릴레이션의 열, 애트리뷰트
      파일 관리 시스템 관점에서 필드에 대응
투플 : 릴레이션의 행
      파일 관리 시스템 관점에서 레코드 대응
도메인 : 하나의 속성이 가질 수 있는 모든 값의 집합
       속성 값을 입력 및 수정할 때 적합성의 판단 기준이 됨
       일반적으로 속성의 특성을 고려한 데이터 타입으로 정의
널 : 속성 값을 아직 모르거나 해당되는 값이 없음을 표현
차수 : 하나의 릴레이션에서 속성의 전체 개수
카디널리티 : 하나의 릴레이션에서 투플의 전체 개수
```

* 릴레이션의 구성 중 릴레이션 스키마는 무엇인가요?

```
릴레이션의 논리적 구조입니다.
릴레이션의 이름과 릴레이션에 포함된 모든 속성 이름으로 정의합니다.
e.g 고객(고객아이디, 고객이름, 나이, 등급, 직업, 적립금)
릴레이션 내포라고도 합니다.
정적인 특징이 있습니다.
```

* 릴레이션 구성 중 릴레이션 인스턴스는 무엇인가요?

```
어느 한 시점에 릴레이션에 존재하는 투플들의 집합입니다.
릴레이션 외연이라고도 부릅니다.
동적인 특징이 있습니다.
```

* 데이터베이스 구성에서 데이터베이스 스키마와 데이터베이스 인스턴스는 무엇인가요?

```
릴레이션이 하나 하나 테이블을 이야기한다면 데이터베이스는 릴레이션을 다 모아둔 전체 구조라고 생각하면 됩니다.
데이터베이스의 전체 구조이자 릴레이션 스키마의 모음입니다.

데이터베이스 인스턴스는 데이터베이스를 구성하는 릴레이션 인스턴스의 모음입니다.
```

* 릴레이션의 특성은 무엇이있나요?

```
투플의 유일성 : 하나의 릴레이션에는 동일한 투플이 존재할 수 없습니다.
투플의 무순서 : 하나의 릴레이션에는 투플 사이의 순서는 의미가 없습니다.
속성의 무순서 : 하나의 릴레이션에는 속성 사이의 순서는 의미가 없습니다.
속성의 원자성 : 속성 값은 원자 값만 사용 할 수 있습니다.
```

* 키가 무엇인가요?

```
릴레이션에서 투플을 유일하게 구별하는 속성 또는 속성들의 집합입니다.
키의 종류에는 기본 키, 후보 키, 슈퍼 키, 외래 키, 대체 키가 존재합니다.
```

* 키의 특성은 아시나요?

```
유일성과 최소성이 있습니다.
유일성은 하나의 릴레이션에서 모든 투플은 서로 다른 키 값을 가져야 합니다.
최소성은 꼭 필요한 최소한의 속성들로만 키를 구성해야 합니다.
```

* 슈퍼 키가 무엇인가요?

```
슈퍼 키는 유일성을 만족하는 속성 또는 속성들의 집합입니다.
최소성은 만족하지 않을 수도 있으나 유일성을 만족합니다.
```

* 후보 키가 무엇인가요?

```
후보 키는 유일성과 최소성을 만족하는 속성 또는 속성들의 집합입니다.
```

* 기본 키가 무엇인가요?

```
후보키 중에서 기본적으로 사용하기 위해 선택한 키입니다.
널 값을 가질 수 있는 속성이 포함된 후보 키는 부적합하며 값이 자주 변경될 수 있는 속성이 포함된 후보 키는 부적합하며, 단순한 후보키를 선택합니다.
```

* 대체 키는 무엇인가요?

```
기본 키로 선택되지 못한 후보 키입니다.
```

* 외래 키가 무엇인가요?

```
다른 릴레이션의 기본 키를 참조하는 속성 혹은 속성들의 집합입니다.
릴레이션들 간의 관계를 표현합니다.
  참조하는 릴레이션 => 외래 키를 가지는 릴레이션
  참조되는 릴레이션 => 외래 키가 참조하는 기본 키를 가진 릴레이션
외래 키 속성과 그것을 참조하는 기본키 속성의 이름은 달라도 되지만 도메인은 같아야 합니다.
외래 키는 하나의 릴레이션에는 여러 개가 존재할 수도 있고 외래 키를 기본 키로 사용할 수도 있습니다.
```

* 무결성 제약 조건에 아시나요?

```
데이터의 무결성을 보장하고 일관된 상태를 유지하기 위한 규칙
무결성은 데이터를 결함이 없는 상태, 즉 정확하고 유효하게 유지하는 것입니다.

무결성 제약 조건에는 개체 무결성, 참조 무결성, 도메인 무결성, 고유 무결성, NULL 무결성, 키 무결성이 있습니다.
개체 무결성 제약 조건은 기본 키를 구성하는 모든 속성은 널을 가질 수 없으며,
참조 무결성 제약 조건은 외래 키는 참조할 수 없는 값을 가질 수 없습니다. 즉, 외래키 값은 null 이거나 릴레이션의 기본 키 값과 동일해야합니다.
도메인 무결성 제약 조건은 특정 속성의 값이 그 속성이 정의된 도메인에 속한 값이어야 합니다.
고유 무결성 제약 조건은 특정 속성에 대해 고유한 값을 가지도록 조건이 주어지는 경우, 그 속성값은 모두 달라야하는 제약 조건입니다.
NULL 무결성 제약 조건은 특정 속성값에 NULL 값이 올 수 없다는 조건이 주어진 경우, 그 속성값은 NULL 값이 올 수 없다는 것입니다.
키 무결성 제약 조건은 한 릴레이션에는 최소한 하나의 키가 존재해야한다입니다.
```