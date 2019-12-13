---
title: "[Error]DTO안에 List가 있는 경우 DTO로 파라미터를 넘길때 문제"
date: 2019-12-12
categories:
- Etc
tags:
- Error
- List in DTO
comments : true
---

### 에러상황
ajax post로 넘기고, spring @ResponseBody활용해서 바로 dto로 받을 때(dto 안에 list가 있는 경우) 이런 에러가 발생했다.                    

```xml
12월 13, 2019 10:16:15 오전 org.apache.catalina.core.StandardWrapperValve invoke
심각: 경로 []의 컨텍스트 내의 서블릿 [dispatcher]을(를) 위한 Servlet.service() 호출이, 근본 원인(root cause)과 함께, 예외 [Request processing failed; nested exception is org.springframework.beans.InvalidPropertyException: Invalid property 'taCostBudgList[0][budg10]' of bean class [com.hsinfo.mvc.domain.task.TaCostBudgWrapper]: Property referenced in indexed property path 'taCostBudgList[0][budg10]' is neither an array nor a List nor a Map; returned value was [400000]]을(를) 발생시켰습니다.
org.springframework.beans.InvalidPropertyException: Invalid property 'taCostBudgList[0][budg10]' of bean class [com.hsinfo.mvc.domain.task.TaCostBudgWrapper]: Property referenced in indexed property path 'taCostBudgList[0][budg10]' is neither an array nor a List nor a Map; returned value was [400000]
```

처음에는 budg10 부터 걸리는게 이상해서.. 타입 문제로 잠깐 생각하고 살펴보다가.. 그게 아니라는 걸 깨달았다..         



### 원인         
arr = [{element:value}, ...] 형식으로 되어있는 배열을 POST로 넘길 때.      

array[index].element = value 이런 식으로 넘어가야 하는데,         

array[index][element] = value 이렇게 전송이 되어서 생기는 문제이다.                      

>List\<DTO\> 는 list[0].budg10 은 사용가능한데, list[0][budg10]는 사용이 불가능하다.            
List\<map\> 은 둘 다 사용가능하다...


### 해결방법

반복 돌리면서 일일히 다시 arr[i].element : value;        

이렇게 다시 적어줘서 해결한다는 글도 봤고,        
(물론 해결이야 하겠지만.. 그럼 또 똑같은거 두 번씩 써줘야 하니까.. )          
[출처](http://www.bmchild.com/2014/02/spring-mvc-3-property-referenced-in.html)   


wrapper 클래스를 다시 만들기도 해보고, JSON 변환도 하고 다 해봤는데 해결이 안되어서,        
[출처](https://stackoverflow.com/questions/23012841/receiving-json-and-deserializing-as-list-of-object-at-spring-mvc-controller)         
(똑같은데.. 그대로 같은 에러가 발생해서 아마 현재 버전에서는 개선이 되었겠죠.....)           
     
우선은 .[element] 로 쓰나 .element 로 쓰나 상관 없이 잘 받는 Map으로 사용했다..             


결론.    
프론트 딴은 코드 바꾼거 아무것도 없이 컨트롤러 파라미터만        
"List<DTO>" 대신 "List<Map>"을 사용했다...    
아주 잘 받아온다...          
  



