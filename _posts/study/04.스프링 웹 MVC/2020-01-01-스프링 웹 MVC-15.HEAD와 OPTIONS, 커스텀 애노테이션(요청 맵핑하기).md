---  
title: "스프링 웹 MVC-15.HEAD와 OPTIONS, 커스텀 애노테이션(요청 맵핑하기)"
date: 2020-01-01
categories: 
- Back-end
tags:
- Spring 
- Spring MVC
comments : true
---

## HTTP 요청 맵핑하기 5부: HEAD와 OPTIONS 요청 처리
우리가 구현하지 않아도 스프링 웹 MVC에서 자동으로 처리하는 HTTP Method
- HEAD
- OPTIONS

HEAD
- GET 요청과 동일하지만 응답 본문을 받아오지 않고 응답 헤더만 받아온다.

OPTIONS
- 사용할 수 있는 HTTP Method 제공
- 서버 또는 특정 리소스가 제공하는 기능을 확인할 수 있다. 
- 서버는 Allow 응답 헤더에 사용할 수 있는 HTTP Method 목록을 제공해야 한다.

참고
- https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html
- https://github.com/spring-projects/spring-framework/blob/master/spring-test/src/test/java/org/springframework/test/web/servlet/samples/standalone/resultmatchers/HeaderAssertionTests.java
