---
title: "[보안] CSRF(Cross Site Request Forgery)란 무엇인가?"
date: 2018-07-14 17:34:18
categories:
    - 보안
tags: 
- 보안
- CSRF
---

# CSRF란 무엇인가?
~~~
CSRF(Cross Stie Request Forgery) : 사이트간 요청 위조
웹 애플리케이션 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격방법을 의미한다 - 나무위키
~~~

정의만 보면 앞서 알아봤던 `XSS`와 `SQL injection`과 비슷하다.
`XSS`가 사용자가 특정 사이트를 신뢰한다는 점을 공격하는거라면, `CSRF`는 특정 사이트가 사용자의 브라우저를 신뢰한다는 점을 공격하는 것이 다르다.

간단하게 정리하자면, 악성코드가
XSS: 클라이언트에서 발생 / CSRF: 서버에서 발생
이라고 할 수 있다.

2008년도에 있었던 옥션 해킹 사고도 CSRF로 공격을 했다고 한다. (해커가 옥션 운영자에게 CSRF 코드가 포함된 이메일을 보내서 관리자 권한을 얻어냈다)

# 공격 과정

~~~
...
<img src="http://auction.com/changeUserAcoount?id=admin&password=admin" width="0" height="0">
...
~~~

위 옥션 사건을 예로 들어보자.

0. 옥션 관리자 중 한명이 관리 권한을 가지고 회사내에서 작업을 하던 중 메일을 조회한다. (로그인이 이미 되어있다고 가정하면 관리자로서의 유효한 쿠키를 갖고있음)
1. 해커는 위와 같이 태그가 들어간 코드가 담긴 이메일을 보낸다. 관리자는 이미지 크기가 0이므로 전혀 알지 못한다.
2. 피해자가 이메일을 열어볼 때, 이미지 파일을 받아오기 위해 URL이 열린다.
3. 해커가 원하는 대로 관리자의 계정이 id와 pw 모두 admin인 계정으로 변경된다.

# 방어 방법

#### 1. Referrer 검증
request header에 있는 요청을 한 페이지의 정보가 담긴 referrer 속성을 검증하여 차단.
일반적으로 이 방법만으로도 대부분 방어가 가능할 수 있다.
옥션이 아닌 개인 이메일에서 요청이 들어오는 것처럼,
같은 도메인 상에서 요청이 들어오지 않는다면 차단하도록 하는 것이다.


#### 2. CSRF Token 사용

랜덤한 수를 사용자의 세션에 저장하여 사용자의 모든 요청(Request)에 대하여 서버단에서 검증하는 방법.


~~~
// 로그인시, 또는 작업화면 요청시 CSRF 토큰을 생성하여 세션에 저장한다. 
session.setAttribute("CSRF_TOKEN",UUID.randomUUID().toString()); 

// 요청 페이지에 CSRF 토큰을 셋팅하여 전송한다 
<input type="hidden" name="_csrf" value="${CSRF_TOKEN}" />
~~~

#### 3. CAPTCHA 사용

요즘은 거의 모든 웹사이트에서 캡차를 이용하는 것 같은데 캡차이미지상의 숫자/문자가 아니라면 해당 요청을 거부하는 것이다.



이 외에도 form 태그를 입력할 시 GET방식을 지양하고 POST방식을 쓰도록 하는 것은 기본이라고 할 수 있다.

---
출처 : [나무위키](https://namu.wiki/w/CSRF), [위키피디아](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0),
http://blog.ilkyu.kr/entry/CSRF-Crosssite-Request-Forgery-%EC%82%AC%EC%9D%B4%ED%8A%B8-%EA%B0%84-%EC%9A%94%EC%B2%AD-%EC%9C%84%EC%A1%B0,
http://itstory.tk/entry/CSRF-공격이란-그리고-CSRF-방어-방법