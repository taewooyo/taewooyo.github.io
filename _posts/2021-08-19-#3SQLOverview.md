---
layout: post
title: "[QnA] SQL"
categories: Database
author: bn-tw2020
---
* content
{:toc}


## Intro

```
SQL 개요를 정리합니다.
```





---

* SQL 개요

```
SQL은 현재 DBMS 시장에서 관계 DBMS가 압도적인 우위를 차지하는데 중요한 요인의 하나입니다.
관계 대수와 관계 해석을 기반으로, 집단 ㅎ마수, 그룹화, 갱신 연산 등을 추가하여 개발된 언어입니다.

SQL은 비절차적 언어이므로 사용자는 자신이 원하는 바만 명시하여, 원하는 것을 처리하는 방법은 명시할 수 없습니다.
관계 DBMS는 사용자가 입력한 SQL문을 번역하여 사용자가 요구한 데이터를 찾는데 필요한 모든 과정을 담당합니다.
자연어에 가까운 구문을 사용하여 질의를 표현할 수 있으며, 대화식 SQL, 내포된 SQL이 존재합니다.
```

* SQL 구성요소 알려주세요.

```
SQL은 Structured Query language입니다.
위에서 설명한 2가지 인터페이스인 대화식, 내포된 SQL있습니다.
구성요소는 3가지이며, 데이터 정의어, 데이터 조작어, 데이터 제어어입니다.
```

> 이 글에서 데이터 조작어인 select, insert, update, delete 등의 삭제 sql 문법은 따로 찾아보시는게 빠를듯 합니다.

* 데이터 정의어의 종류에 대해 설명해주세요.

```
스키마의 생성과 제거 : 동일한 데이터베이스 응용에 속하는 릴레이션, 도메인, 제약조건, 뷰, 권한 등을 그룹화하기 위해서 스키마 개념을 지원합니다.
                  CREATE SEHEMA MY_DB AUTHORIZATION nam;
                  DROP SEHEMA MY_DB RESTRICT;
                  DROP SEHEMA MY_DB CASCADE;

CREATE
    DOMAIN 도메인 생성
    TABLE  테이블 생성
    VIEW   뷰 생성
    INDEX  인덱스 생성
ALTER
    TABLE  테이블의 구조를 변경
DROP
    DOMAIN 도메인 제거
    TABLE  테이블 제거
    VEIW   뷰 제거
    INDEX  인덱스 제거
```

* 참조 무결성 제약조건 유지를 위해 사용되는 명령어 아시나요?

```
ON DELETE NO ACTION : 아무것도 하지 말아라
ON DELETE CASCADE : 연쇄적으로 해라
ON DELETE SET NULL : NULL로 해라
ON DELETE SET DEFAULT : DEAFULT로 해라

ON UPDATE NO ACTION
ON UPDATE CASCADE
ON UPDATE SET NULL
ON UPDATE SET DEFAULT
```

* 조인

```
조인은 두 개 이상의 릴레이션으로부터 연관된 투플들의 결합입니다.
기본적인 형태는 SELECT ... FROM R, S WHERE R.A <비교 연산자> S.B;
```

* 중첩 질의(subQuery) 

```
외부 질의의 WHERE절에 다시 SELECT ... FROM ... WHERE 형태로 포함된 SELECT 문
insert, update, delete 에도 사용 가능
e.g SELECT EMPNAME, TITLE FROM EMPLOYEE WHERE TITLE = (SELECT FROM EMPLOYEE WHERE EMPNAME = '홍길동')
    홍길동과 같은 직급을 갖는 모든 사원들의 이름과 직급을 출력합니다.

위의 예제와 같이 스칼라 값(단일 값)을 반환하면 상관없지만 중첩 질의의 결과로 한 개의 애트리뷰트로 이루어진 다수의 투플들이 반환될 수 있습니다.
외부 질의의 WHERE절에서 IN, ANY(SOME), ALL, EXISTS와 같은 연산자를 사용해야 합니다. 
```

* 상관 중첩 질의

```
상관 중첩 질의는 중첩 질의의 WHERE 절에 있는 프레디키트에서 외부 질의에 선언된 릴렝션의 일부 애트리뷰트를 참조하는 질의입니다.
```

* INSERT

```
기존의 릴레이션에 투플을 삽입합니다.
참조되는 릴레이션에 투플이 삽입되는 경우에는 참조 무결성 제약 조건의 위배가 발생하지 않지만,
참조하는 릴레이션에 투플을 삽입되는 경우에는 참조 무결성 제약 조건을 위배할 수 있습니다.
기본적인 형태는 INSERT INTO 릴레이션(속성1, 속성2 ...) VALUES (값1, 값2 ...);
```

* DELETE

```
삭제 연산은 한 릴레이션으로부터 한 개 이상의 투플들을 삭제합니다.
참조되는 릴레이션의 삭제 연산의 결과로 참조 무결성 제약 조건이 위배될 수 있으나,
참조하는 릴레이션의 삭제 연산의 결과로 참조 무결성 제약조건을 위배하지는 않습니다.
기본적인 형태는 DELETE FROM 릴레이션 WHERE 조건;
```

* UPDATE

```
한 릴레이션에 들어있는 투플들의 애트리뷰트 값들을 수정합니다.
기본 키나 외래 키에 속하는 애트리뷰트의 값이 수정되면 참조 무결성 제약 조건에 위배될 수 있습니다.
기본적인 형태는 UPDATE 릴레이션 SET 애트리뷰트 = 값 WHERE 조건;
```

* 트리거가 무엇인가요?

```
명시된 이벤트(데이터베이스의 갱신)가 발생할 때마다 DBMS가 자동적으로 수행하는, 사용자가 정의하는 문(프로시저)입니다.
데이터베이스의 무결성을 유지하기 위한 일반적이고 강력한 도구입니다.
트리거를 이벤트-조간-동작(ECA) 규칙이라고도 부릅니다.
E(Event) : 트리거를 활성화 시키는 사건(INSERT, UPDATE, DELETE)
C(Condition) : 트리거가 활성되었을 때 확인하는 조건(WHERE 문에 쓰이는 모든 조건문)
A(Action) : Condition이 참이면 수행되는 쿼리문

참조 무결성은 시스템이 정의해놓는 트리거이며, 사용자가 트리거를 만들 수 있다는 것을 의미합니다.
연쇄적으로 활상화시켜 트리거를 진행 시킬 수도 있습니다.

e.g 새로운 사원 추가 시, 급여가 1500000 이하면 급여 10% 인상
    CREATE TRIGGER RAISE_SALARY 
    AFTER INSERT ON EMPLOYEE
    REFERENCING NEW AS newEmployee 
    FOR EACH ROW 
    WHEN (newEmployee.SALARY < 1500000) 
    UPDATE EMPLOYEE
    SET newEmployee.SALARY = SALARY * 1.1 
    WHERE EMPNO = newEmployee.EMPNO; 
```

* assertion(주장)이 무엇인지아시나요?

```
트리거는 제약 조건을 위반했을 때 수행할 동작을 명시하는 것이고, 주장은 제약조건을 위반하는 연산이 수행되지 못하도록 하는 것입니다.

e.g STUDENT에 없는 학생의 학번이 ENROLL 릴레이션에 나타나지 않도록 합니다.
    CREATE ASSERTION EnrollStudentIntegrity
    CHECK (NOT EXISTS
        (SELECT *
          FROM ENROLL
          WHERE STNO NOT IN
          (SELECT STNO FROM STUDENT));
```

* 커서가 무엇인가요?

```
SELECT는 단일 투플도 가져오고 여러 투플을 가져옵니다.
호스트 언어는 단일 변수/레코드 위주의 처리(투플 위주의 방식)를 지원하는 반면에 SQL은 집합 위주의 방식를 지원하기 때문에 데이터 불일치가 발생합니다.
불일치 문제를 해결하기 위해서 커서가 사용됩니다.
커서는 한 번에 한 투플씩 가져옵니다.

```

