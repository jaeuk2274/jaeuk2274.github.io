---
title: Spring Triangle-03.스프링 PSA
date: 2019-12-05
categories:
- Back-end
tags:
- Spring
- PSA
comments : true
---


## 스프링 PSA
### PSA?
우선 참고 문서를 보면
>PSA(Portable Service Abstraction)란 환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하려는 추상화 구조를 말한다. 
이는 POJO 원칙을 철저히 따른 Spring 의 기능으로 Spring 에서 동작할 수 있는 Library 들은 POJO 원칙을 지키게끔 PSA 형태의 추상화가 되어있음을 의미한다. 
가령 일반적인 JUnit이나 Mybatis 등의 여러 Java Framework 에서 사용가능한 라이브러리들은 Spring 에서 지원하는 JUnit 이나 MyBatis 라이브러리와 다르다. 또한 JPA를 사용할 때에 있어서도 Spring JPA가 추상화 시켜준다.      
따라서 개발자가 Hibernate를 사용하건 EclipseLink 를 사용하건 신경쓸 필요가 없다.       
따라서 이러한 외부 라이브러리들은 Spring에서 사용할 때 내부구현이 달라지더라도 동일한 인터페이스로 동일한 구동이 가능하게끔 설계되어 있으며 의존성을 고려할 필요가 없다.       
Spring 은 이렇듯 특정 기술에 직접적 영향을 받지 않게끔 객체를 POJO 기반으로 한번씩 더 추상화한 Layer 를 갖고 있으며 이를통해 일관성있는 서비스 추상화를 구현한다.      
  
그냥 쉽게 말하면 잘 만든 인터페이스로 이해하면 될 것 같다.       
스프링의 철학처럼, 비침투성, POJO 등..    
내가 어떤 코드를 짰는데, 그게 JPA를 쓰던, MyBatis 쓰던 코드의 변경이 없어도 된다. 어떤 특정 기술에 특화된 코드가 아니다.     
(잘 만든 인터페이스를 사용하기 때문에)     

즉 확장성이 좋고, 기술에 특화되어 있지 않다는 말이다.

Service Abstraction(서비스 추상화) 참고문서 : https://en.wikipedia.org/wiki/Service_abstraction                


몇 가지 예들을 살펴보면, 

첫번째로 스프링 웹 MVC            
@Controller 와 @RequestMapping
기존 코딩은       
@Controller | @ReuqestMapping | ... 등 활용해서 뒤의 url 요청시 이 메서드가 처리하게끔 매핑을 하게 코딩이 되어 있다.             
      
      
#### webflux
그런데 스프링 5 새로운 프로그래밍 모델이 나왔는데,    
스프링 웹 MVC 와 거의 흡사한 방법으로 코딩을 하지만, 기술이 완전히 다르다.         

기존 서블릿 방식처럼 하나의 요청당 하나의 스레드를 사용하는 구조가 아니라     
cpu개수만큼의 스레드를 유지하면서 (뒤딴에서는 스레드풀을 이용하긴 하지만)     
앞딴에서 최소한의 스레드로 가용성을 높이는 기술이 적용된 방식이다.    

설정에서
spring-boot-starter-web
에서
spring-boot-starter-webflux
로 바꿔 주면 된다.
-> 대신 modelAndView 사용 불가        

이런 방법으로 바꾸면 네티 기반으로 실행이 된다.     

스프링 웹 MVC 추상화 계층을 사용하지만, 실제로는 네티를 쓰는 것이다.         
여러 기반시설 등 서블릿으로 코딩할수도 있고 리엑티브로 코딩할수도 있다.    

기존 @Controller | @ReuqestMapping 방식으로 코딩한 소스를 바꿀 필요가 없다.         
또 서버도 톰캣, 제티, 네티, 언더토우 어떤 걸 사용하던지 코드의 변경이 없다.           
  
즉, 스프링 웹 MVC도 PSA중 하나이다.     

두번째로는 스프링 트랜잭션
스프링의 추상화 계증 트랜잭션을 사용할 때     
>@Transactional               
PlatformTransactionManager            
JpaTransacionManager | DatasourceTransactionManager | HibernateTransactionManager                

PlatformTransactionManager를 사용하는데 이는 역시 마찬가지로     
JDBC를 쓸 수도 있고, JPA 사용하는 경우, 하이버네이트 등..          
사용하는 경우에 기술에 따라서 다르게 바꾸더라도 코드는 변경하지 않고 사용할 수 있다.         
스프링 트랜잭션 어노테이션도 여러가지 다양한 기술로 바꿔쓸 수 있는 PSA이다.           

세번째는 스프링 캐시      
스프링 캐시를 사용할 때 (CacheManager)    
>@Cacheable | @CacheEvict | ...   
CacheManager       
JCacheManager | ConcurrentMapCacheManager | EhCacheCacheManager | ...   

역시 마찬가지로 캐시도 여러 가지가 있는데(ehcache 등)
어노테이션만 쓰면 상관없이 코드 변경 없이 사용이 가능하다.     
마찬가지로, 기술이 바뀌더라도 코드는 바뀔 이유가 없어진다.            




