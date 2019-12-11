---
title: 스프링 공식 예제 프로젝트 PetClinic
date: 2019-12-04
categories: Back-end
tags: 
- Spring 
- PetClinic 
- Thymeleaf 
- Jar 
- War
comments : true
---

## 들어가며
스프링을 활용 웹 개발을 주로 하긴 하지만..     
단순 구현이 아니라, 자세한 동작 원리부터 시작해서 깊게 한번 공부를 해 보고 싶다는 생각이 들었다.     
(사실 입사하자마자 난 인정받는 에이스 개발자가 될거야! 하며 뿜뿜 열심히 그때 당시 도움이 될만한 공부들을 했는데,     
기록하지 않고 혼자 공부하니 까먹는 것들이 생기더라.. 나중에 내가 정리한 걸 찾아보기도 어렵고..      
또 대학시절 블로그를 운영하며, 정리하며 하게 되는 공부의 중요성, + 기타 나중에 찾아볼때 등 장점을 알고 있기 때문에  
물론 그냥 공부한 하는것보단 훨씬 많은 시간이 소요될 테지만, 내 시간을 좀 더 추가해서 깊고, 오래가는 공부를 꾸준히 하려고 한다.      
스프링 뿐만 아니라 꾸준히 계속해서 다른 분야까지도..

대학시절 블로그(현재는 미운영) : https://jaeuk2274.tistory.com/
- 핵심 프로젝트고 가장 많은 열정을 쏟았던 프로젝트(투자관제탑)은 글도 가장 많지만     
아무래도 실제 상용화된 시스템이라, 고객께서 글들을 다 내려달라고 하셨다.     
그 부분이 조금 아쉽긴 하지만..       

아무튼 돌아와서     
현재 운영 시스템 중 메인인 것은 앵귤러 + 스프링4 + 마이바티스 로 구현되어 있는 시스템을 운영하는데,     
스프링 시큐리티도 구현되어 있지만, 아는 만큼 보인다고..     
이미 구현된 걸 바탕으로 단순 구현이나 활용은 가능하지만 내가 정말 자세하게 이해하고 있나? 란 생각에는 의문이 들었다.    

나름 대학 시절에 토비의 스프링이나 뭐 기타 스프링 책들 공부해보긴 했지만(그냥 읽어보기만.. 지금 다시보면 보이는게 많은 것 같다..)     
스프링 시큐리티, 클라우드, 현재의 부트까지 기타 최신 사용하는 기술들 혼자서 자기계발 겸 공부하고,    
또 내가 잘 안다면 업무에 적용할 수도 있을 것 같다는 생각이 들었다.     

또 여기에는 환경상 전 시스템이 모두 자바 1.6으로 이루어져 있는데,       
자율적으로 오픈JDK 1.8로 올려본 한 경험이 있어서 그런 경향도 있는 것 같다.      
프레임워크 딴도 충분히 내가 공부하고 이해하면 적용시켜 활용할 수 있을 것 같다.     

그래서 나름대로 자기계발 커리큘럼을 짜 보았고,     
우선 스프링은 평소 좋아하는 개발자인 백기선님의 로드맵을 따라서 차근차근 공부해 나가면 될 것 같다.     
이후에 다른 분야까지도 꾸준하게!

## 스프링 예제 프로젝트 PetClinic 

1. 프로젝트 설정
2. 프로젝트 살펴보기
3. 프로젝트 과제풀이

### IntelliJ
솔직히 유료기 때문에     
회사에서는 이클립스 혹은 STS만 사용해 왔는데 앞으로의 개발자로써의 성장(생산성), 공부하는 강의에서도 활용하기도 하고,     
그리고 말로만 들어오던 걸 경험해 보기 위해, 체험 버전 후 구매할 예정이다.     
또 개인구매 라이센스더라도 회사에서 사용할 수 있다고 하더라. 굳!     


### 프로젝트 세팅
스프링 공식 프로젝트 펫 클리닉          
깃허브 주소 :  https://github.com/spring-projects/spring-petclinic    

들어가서 깃 클론 주소 복사하여     
인텔리제이 자체에서 링크만 넣고 바로 깃 클론 해서 프로젝트 생성할 수 있게 지원하더라.    
정말 편한 것 같다.    

readme 읽어보면 단 4줄로 세팅하는 설명이 나와있다.   
![readme](https://github.com/jaeuk2274/jaeuk2274.github.io/blob/master/_posts/img/%EC%98%88%EC%A0%9C%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%9E%85%EB%AC%B8(%EA%B0%9C%EC%A0%95%ED%8C%90)/01.Readme.png?raw=true)

> 1. 깃 클론    
2. 경로 이동    
3. 메이븐 패키지    
4. 자르파일 실행    

인텔리제이에서는 자체 터미널도 지원하고     
(기본 경로는 기본 홈 디렉토리)    

난 인텔리 자체 기능으로 깃 클론 받았기 때문에,    
터미널 켜고    
기본 경로기 때문에 그냥 바로 메이븐 패키지 실행     
(package : 개발자가 선택한 war, jar 등의 패키징 수행)    

현재 이 프로젝트의 pom.xml에 보면     
패키징 설정이 되어 있지 않다.     
> ex. <packaging> war </packaging>     
아무것도 미지정시에는 디폴트가 jar 이다.    

### jar와 war의 차이점?

학생때 카페24 통해서 호스팅을 해본 경험으로는 war파일로 올렸던 것 같은데,
정확한 차이점을 몰라서 이번에 공부해 보았다.

> WAR: Web Application Archive    
JAR: Java Archive    


>.jar files: The .jar files contain libraries, resources and accessories files like property files.      
.war files: The war file contains the web application that can be deployed on any servlet/jsp container. The .war file contains jsp, html, javascript and other files necessary for the development of web applications.

즉, war가 조금 더 큰 범위이다.
jar에 포함된 것들은 war에 포함되며,
war는 추가적으로 서블릿, 웹 관련 파일, xml 등 웹 어플리케이션에 활용되는 모든 자원을 함께 압축한 것으로 보면 될 것 같다.

아마 뒤에서도 설명하겠지만,
스프링 부트는 웹 프로젝트임에도 불구하고, java main 빌드로 실행이 가능하다.
그래서 아마 jar로도 실행이 가능하지 않을까..(추측)

이 부분은 추후 부트에 대해서 공부하면서 더 공부하고 찾아볼 예정이다.

무튼 패키징을 완료하면 jar 가 어디에 떨어지는지 빌드에 찍힌 로그가 보인다.

![jar경로](https://github.com/jaeuk2274/jaeuk2274.github.io/blob/master/_posts/img/%EC%98%88%EC%A0%9C%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%9E%85%EB%AC%B8(%EA%B0%9C%EC%A0%95%ED%8C%90)/03.Bulid.png?raw=true)

Java -jar jar파일 실행      
target/*.jar 경로에 있는 jar파일 

빌드되면 로그에도 찍히지만 8080포트로 실행된 것이 보인다.
localhost:8080

![실행](https://github.com/jaeuk2274/jaeuk2274.github.io/blob/master/_posts/img/%EC%98%88%EC%A0%9C%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%9E%85%EB%AC%B8(%EA%B0%9C%EC%A0%95%ED%8C%90)/04.%E1%84%89%E1%85%B5%E1%86%AF%E1%84%92%E1%85%A2%E1%86%BC.png?raw=true)

종료는 컨트롤 c로 실행 종료 가능하다(인텔리j 단축키)      
아직은 인텔리j 단축키를 외우지 못해서 하나하나 찾아가면서 쓰고 있다.     
나중에 한번 정리해서 올려놔야겠다.    

> 가장 자주쓰는 단축키       
파일 찾기 : 컨트롤 + 쉬프트 + O     

그리고 한번 메이븐 패키징한 후에는    
앞서 말했든    
웹 프로젝트 임에도 불구하고 java-main에서 직접 실행이 가능하다.    
(부트가 그걸 도와준다            
웹임에도 불구하고 내장 톰캣 및 자동 설정 세팅, 아주 쉽고 빠르게 스프링 기반 뿐만아니라 일반 프로젝트도 빠르게 구성이 가능.        
추후 부트 동작원리부터 공부 예정)    


역시나 스프링 공식 프로젝트 답게 잘 되어 있다. 소스도 깔끔하고.   
일반적인 메이븐 프로젝트 구조에,      
도커 관련 설정도 있었고, junit5기반 테스트 소스도 들어있다.   
(한번 제대로 보면 좋은 공부가 될 것 같다.)   


프로젝트 파악시     
application.properties -> 로깅 옵션 디버그 변경

``` xml
# Logging
# logging.level.org.springframework=INFO
logging.level.org.springframework.web=DEBUG
```

보통 pom.xml에 log4j 넣고 설정해주었는데,     
부트에서는 정말 손쉽게 로그 관리를 하는구나.. 느꼈다.


디스패처 서블릿 최초 초기화 되는 것도 보이고
``` terminal
2019-12-04 22:35:18.042  INFO 2516 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
```

내가 지금까지 자세히 안 본 건가
o.s.web.servlet 
-> org.springframework.web.servlet

약어로 쓰이고 있었구나..
처음 자세히 본 것 같다.. 또 하나의 발견..!

디버그 모드도 이클립스랑 크게 다를 바 없으나,
훨씬 빠르고, 더 보기 좋게 UI가 구성되는 것 같다..

### 과제

1. lastname 검색 -> firstname 검색
2. 완벽 일치가 아닌 포함하는 것도 같이 검색
3. 오너-나이 추가(CRU)

``` java
jpa
// find owners by last name
// Collection<Owner> results = this.owners.findByLastName(owner.getLastName());
Collection<Owner> results = this.owners.findByLastNameAndFirstName(owner.getLastName(), owner.getFirstName());
if (results.isEmpty()) {
    // no owners found
    result.rejectValue("lastName", "notFound", "not found");
    return "owners/findOwners";
} else if (results.size() == 1) {
    // 1 owner found
    owner = results.iterator().next();
    return "redirect:/owners/" + owner.getId();
} else {
    // multiple owners found
    model.put("selections", results);
    return "owners/ownersList";
}
```   
``` java     
@Query("SELECT DISTINCT owner FROM Owner owner left join fetch owner.pets WHERE owner.lastName LIKE %:lastName%")
@Transactional(readOnly = true)
Collection<Owner> findByLastName(@Param("lastName") String lastName);

@Query("SELECT DISTINCT owner FROM Owner owner left join fetch owner.pets WHERE owner.lastName LIKE %:lastName% AND owner.firstName LIKE %:firstName%")
@Transactional(readOnly = true)
Collection<Owner> findByLastNameAndFirstName(@Param("lastName") String lastName, @Param("firstName") String firstName);
```

1,2
쉬운 과제라 모두 쉽게 완료하고,

3 
create를 보고 따라가다가 
JPA를 처음 사용해 봤는데 모델링만 잘 되어 있으면 정말 편할 것 같았다.      
vo를 테이블로 보고 변수에 칼럼을 바로 매칭해서 사용하는 것도 정말 편리하고,       
불필요한 소스들도 굉장히 많이 줄일 수 있을 것 같다.      

jpa도 빨리 공부해보고 싶다.     


### 타임리프(Thymeleaf)?

좀 찾아보니 가장 피부로 느껴질만한 장점은 이것인것 같다. (서버 미구동시에도 확인 가능)   

> 타임리프(Thymeleaf)는 Freemarker, Velocity와 같은 View Template Engine 입니다.     
하지만 타임리프는 다른 템플릿 엔진들과는 차별화되는 장점을 지니고 있습니다.
1. 서버상에서 동작하지 않아도 된다.
2. 전체적인 마크업 구조를 흐트려트리지 않는다.

> Freemarker나 Velocity같은 템플릿엔진들은 해당 템플릿이 지원해주는 다양한 기능(if, for, switch 같은)을 통해 마크업을 진행하지만, 결과물을 확인할때는 서버를 구동시킨 상태에서 결과물을 확인해야 합니다.       
이는 브라우저에서 단순 html 마크업작업을 하는 것보다 더 많은 시간을 소요하게 만듭니다.      
하지만 타임리프는 이러한 단점을 해결하여 마치 static html파일을 작업하듯이 텍스트에디터를 통해 작업하고 해당내용을 브라우저로 바로 확인할 수 있습니다.   
그 이유는 타임리프가 html 의 속성(attribute)를 기반으로 해서 작성되기 때문입니다.    
html은 각 테그들마다 몇가지 속성을 가지고 있고(style, class, alt 등등) 이 이외의 선언되지 않은(사용자가 임의로 정한) 속성들에 대해서는 브라우저는 별다른 에러를 발생시키지 않고 화면에 표시하지 않습니다.   



프로젝트 진행 중에 프론트는 타임리프로 되어 있다고 하던데, (처음 접해보았다. 예전엔 jsp 사용해왔음)
진행하면서 의문점이 생겼다. 

마침 나와 비슷한 생각을 가진 분의 질답이 있어 가져와 본다. 

>실제 업무에서 thymeleaf를 흔히 사용하는지 궁금합니다.
만약 앵귤러나 리액트를 사용할 때에도 View Template을 사용하는 경우가 있을까요??

>흔히 사용하느냐는 질문에 제가 여러 회사의 속사정을 알고 있는게 아니라 머라 답해야 할지 모르겠네요. 하지만 적어도 새로 만드는 프로젝트는 JSP 보단 타임리프를 많이들 선택할 겁니다. 그런데 질문의 의도는 뷰 전문 기술이 있는데 왜 타임리프를 써야 하냐로 보이네요.

프론트만 전문으로 하는 엔지니어나 팀이 있다면 타임리프를 안써도 되겠지만 서버 개발자가 뷰까지 만들어야 하는 경우라면 JSP 또는 타임리프를 쓰는게 현실적인 선택일거 같습니다. 학습 비용 때문이죠. 그리고 원한다면 리액트나 앵귤러 뷰.js랑 타임리프 같은 뷰 템플릿 엔진이랑 조합해서 쓸 수도 있습니다.

난 학부때부터, 취업하고 나서도 
항상 front, back 구별없이 둘 다 건들면서 
그래도 어느 정도는 프론트딴도 같이 다룰 수 있지만, 보통의 백엔드 서버 개발자의 경우 아니라는 생각도 들었고,
같이 조합하여 사용하더라도 괜찮을 것 같다. 


