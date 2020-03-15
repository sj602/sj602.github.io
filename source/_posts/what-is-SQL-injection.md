---
title: "[보안] SQL injection 이란?"
date: 2018-07-13 19:26:17
categories:
    - 보안
tags: 
- 보안
- SQL injection
---

XSS와 더불어 가장 흔한 보안 공격 기술인 **SQL injection**에 대해서 알아보았다.

# SQL injection 이란?
~~~
SQL 인젝션은 코드 인젝션의 한 기법으로 클라이언트의 입력값을 조작하여 서버의 데이터베이스를 공격할 수 있는 공격방식을 말한다. - 나무위키
~~~

데이터베이스 조작언어인 SQL 입력값에 정상적인 값이 아닌 SQL문을 삽입해 데이터베이스에 해를 가하는 공격이라고 할 수 있다.

예를 들어, 로그인을 하는 코드가 있다고 하자.
~~~
SELECT user FROM user_table WHERE id='입력한 아이디' AND password='입력한 비밀번호';
~~~
위와 같이 SQL문을 짜서 데이터베이스에 존재하는 user_table 테이블에서 입력한 id와 입력한 password와 동일한 값을 찾아 반환해주는 코드이다.

일반적인 유저라면
> id = 홍길동
> password = 1234

이런식으로 입력을 하겠지만

SQl injection을 할 유저는
> id = 홍길동
> password = 1234'; DROP table user_table--

이런식으로 입력을 하게 된다.

그렇게 되면 전체 SQL 구문은 이렇게 되어버린다.

~~~
SELECT user FROM user_table 
WHERE id='홍길동' AND password='1234'; DROP TABLE user_table--';
~~~

SQL에서 ;(세미콜론)은 한 구문의 끝을 나타내므로 위 구문은

~~~
SELECT user FROM user_table WHERE id='홍길동' AND password='1234'
~~~
~~~
DROP TABLE user_table--'
~~~

이렇게 2개의 구문으로 나눠져 실행되게 된다.

--(하이픈 2개) 다음으로 오는 문장은 주석 처리가 되므로,
결국 DROP 명령어에 의해 해당 table은 DB에서 지워지게 된다.

이 외에도 로그인을 무조건 성공하게 하는 
**' OR '1' = '1** (1은 1과 같으니 WHERE절은 무조건 참이 되어 아이디 또는 비밀번호가 무엇이든 로그인 성공)
등 수많은 케이스가 있다.

___
# 방어 방법

#### 1. 입력값 검증

다음과 같은 특수문자 혹은 SQL 명렁문이 입력값에 포함되있는지 검증한 후 차단한다.
~~~
*, –, ‘, “, ?, #, (, ), ;, @, =, *, +, union, select, drop, update, from, 
where, join, substr, user_tables, user_table_columns, 
information_schema, sysobject, table_schema, declare, dual,…
~~~

유저가 클라이언트단에서 자바스크립트를 끌 수 있으니
클라이언트 단에서도 차단 및 서버 단에서도 차단하여 이중장치를 마련하자.
대부분의 경우, 입력값 검증만 해도 막을 수 있다.

#### 2. SQL 오류 발생시 오류 메세지 클라이언트 표시 금지
SQL 오류 메세지를 통하여 유저가 해당 DB의 구조를 알 수 있다.

#### 3. HASH 사용
DB에 데이터 저장시 민감한 정보는 HASH를 이용해 저장한다. 
특히 패스워드는 반드시 SHA-256 이상으로 해싱 후 저장! (하지 않을시 개인정보보호법 29조 위반이라고 한다..)





---
출처 : [나무위키](https://namu.wiki/w/SQL%20injection), http://blog.plura.io/?p=6056, https://docs.microsoft.com/ko-kr/sql/relational-databases/security/sql-injection?view=sql-server-2017, http://asfirstalways.tistory.com/360, http://brownbears.tistory.com/59