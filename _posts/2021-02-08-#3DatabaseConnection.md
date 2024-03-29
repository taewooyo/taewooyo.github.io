---
layout: post
title: "DBMS 접속 기술"
categories: DB
author: bn-tw2020
---
* content
{:toc}

## Intro

```
DBMS접속은 사용자가 데이터를 사용하기 위해 응용 시스템을 이용하여 DBMS에 접근하는 것을 의미한다.
```




---

## DBMS 접속의 개요

* 응용 시스템은 사용자로부터 매개 변수를 전달받아 SQL을 실행하고

* DBMS로부터 전달받은 결과를 사용자에게 전달하는 매개체 역할을 수행

* 인터넷을 통해 구동되는 웹 응용 프로그램은 웹 응용 시스템을 통해 DBMS에 접근

* 웹 응용 시스템은 웹 서버와 WAS로 구성되며,

* 서비스 규모가 작은 경우 웹 서버와 WAS를 통합하여 하나의 서버만으로 운용할 수 있다.


## DBMS 접속 기술

* DBMS 접속 기술은 DBMS에 접근하기 위해 사용하는 API 또는 API의 사용을 편리하게 도와주는 프레임워크 등을 의미

* API
   - 응용 프로그램 개발 시 운영체제나 DBMS 등을 이용할 수 있도록 규칙 등에 대해 정의해 놓은 인터페이스

* 프레임워크
   - '뼈대', '골조'를 의미하는 용어로, 소프트웨어에서는 특정 기능을 수행하기 위해 필요한 클래스나 인터페이스 등을 모아둔 집합체

### JDBC

```
1. Java 언어로 다양한 종류의 데이터베이스에 접속하고 SQL문을 수행할 때 사용되는 표준 API
2. 1997. 2월 썬 마이크로시스템에서 출시
3. Java SE에 포함되어 있으며, JDBC 클래스는 java.sql, javax.sql에 포함되어 있다.
4. 접속하려는 DBMS에 대한 드라이버가 필요

- Java SE는 Java표준안으로, Java의 문법과 기능들을 정의하는 명세서이다.
  개발 도구인 JDK에 포함되어 사용되며, JDBC의 기능들을 정의하는 클래스 파일을 포함하고 있다.
- 드라이버 : 다른 장치나 시스템을 제어하는데 사용되는 프로그램
```
### ODBC

```
1. 데이터베이스에 접근하기 위한 표준 개방형 API
2. 개발 언어에 관계없이 사용이 가능
3. 1992. 9월 마이크로소프트웨어 출시
4. 프로그램 내 ODBC문장을 사용하여 MS-Access, DBase, DB2, Excel, Text 등
   다양한 데이터베이스에 접근이 가능
5. ODBC도 접속하려는 DBMS에 맞는 드라이버가 필요하지만,
6. 접속하려는 DBMS의 인터페이스를 알지 못하더라도 ODBC 문장을 사용하여 SQL를 작성하면
   ODBC에 포함된 드라이버 관리자가 해당 DBMS의 인터페이스에 맞게 연결해주므로
   DBMS의 종류를 몰라도 된다.
```

### MyBatis

```
1. MyBatis는 JDBC 코드를 단순화하여 사용할 수 있는 SQL Mapping 기반 오픈 소스 접속 프레임워크
2. JDBC로 데이터베이스에 접속하려면 다양한 메소드를 호출하고 해제해야 한다.
   MyBatis는 이를 간소화 했고 접속 기능을 더욱 강화
3. SQL 문장을 분리하여 XML 파일을 만들고, Mapping을 통해 SQL을 실행
4. SQL을 거의 그대로 사용할 수 있어 SQL 친화적인 국내 환경에 적합하여 많이 사용
```


