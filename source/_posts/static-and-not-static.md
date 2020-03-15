---
title: "[Javascript] Class에서의 Static이란?"
date: 2018-06-30 22:55:50
categories:
    - Javascript
tags: 
- javascript
- static
---


클래스의 static method는 정적 메소드라고 불리는데 이는 클래스의 인스턴스가 필요없이 호출이 가능한 메소드를 말한다.

예를 들어,

    class Foo {
        bar() {...}

        static baz() {...}
    }

라는 코드가 있을때 bar()는 

    const f = new Foo()
    f.bar() 

위 예시처럼 f라는 Foo의 인스턴스를 선언을 하고 그 인스턴스에서 호출을 해야하지만

baz()는

    Foo.baz()

위 예시처럼 클래스 Foo에서 바로 호출이 가능하다. 
Foo의 인스턴스를 선언하지 않고도 메소드를 호출이 가능하다는 것이다. 

용어의 반대 관계는 아래와 같다.

** static method != instance method **

