---
title: "[Error]DTO안의 List로 파라미터를 넘길때 문제"
date: 2019-12-12
categories:
- Etc
tags:
- Error
- Ajax
- Post
- List in DTO
comments : true
---

```xml
12월 13, 2019 10:16:15 오전 org.apache.catalina.core.StandardWrapperValve invoke
심각: 경로 []의 컨텍스트 내의 서블릿 [dispatcher]을(를) 위한 Servlet.service() 호출이, 근본 원인(root cause)과 함께, 예외 [Request processing failed; nested exception is org.springframework.beans.InvalidPropertyException: Invalid property 'taCostBudgList[0][budg10]' of bean class [com.hsinfo.mvc.domain.task.TaCostBudgAddList]: Property referenced in indexed property path 'taCostBudgList[0][budg10]' is neither an array nor a List nor a Map; returned value was [400000]]을(를) 발생시켰습니다.
org.springframework.beans.InvalidPropertyException: Invalid property 'taCostBudgList[0][budg10]' of bean class [com.hsinfo.mvc.domain.task.TaCostBudgAddList]: Property referenced in indexed property path 'taCostBudgList[0][budg10]' is neither an array nor a List nor a Map; returned value was [400000]
```
