---
title: Spring Triangle-01.스프링 IoC
date: 2019-12-05
categories:
- Spring
tags:
- Spring 
- IoC
- Bean
- DI
comments : true
---

# 시작
본격적인 로드맵 시작으로 예제로 배우는 스프링 입문(개정판)을 스타트했다.        
대충은 아는 개념들이긴 했지만, 예제 특히 코드로써 직접 구현해보기도 하고,               
그냥 매번 쓰는 방식대로 사용만 해오다가 명확하게 어떤 방식으로 구현되는지, 동작 원리나 라이프사이클 그리고 다른 구현 방식들에 대해서도 알게되었다.      
추가적으로 기존에 명확히 정리되지 않던 개념들은 찾아보고 정리하면서 더 좋은 공부가 된 것 같다.             

## Spring Triangle - 자바 스프링 핵심 3대 요소

1. Spring IoC - 제어 역전(IoC, Inversion of Control)    
2. Spring AOP - 관점 지향 프로그래밍(Aspect Oriented Programming)     
3. Spring PSA - 서비스 추상화(Portable Service Abstraction)    


# Spring IoC

IoC (Inversion of Control)  
제어의 역전이란?     
제어의 역전을 설명하기 전에 먼저 알아야 할 것이 있다. 

## DI

DI (Dependency Injection)     
의존성 주입을 먼저 알아야 하는데      
이 의존성이 어떤 의미를 뜻하냐면     

일반 자바 프로젝트에 서는 내가 사용할 의존성을 내가 만들어 사용해야 한다. 

``` java 
class OwnerController {
	private OwnerRepository repo = new OwnerRepository();
}
``` 
OwnerController가(내가) 사용할 의존성(OwnerRepository)을 직접 new로 만들어 사용한다.

즉, 의존성 주입이란 말 그대로 객체간의 의존성을 외부에서 주입을 해 준다는 개념이다.
    
    
    
    
## IoC (Inversion of Control) ?
    
그럼 IoC란 “내가 사용할 의존성을 누군가 알아서 만들어 준다.”는 개념이다.    
    
제어 역전에서는 컨트롤러가 의존성을 직접 관리하는 것이 아니라 아래와 같은 코드로 동작하게 된다.     

``` java 
@Controller
class OwnerController {
    private static final String VIEWS_OWNER_CREATE_OR_UPDATE_FORM = "owners/createOrUpdateOwnerForm";
    private final OwnerRepository owners;
    private VisitRepository visits;

    public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
        this.owners = clinicService;
        this.visits = visits;
    }
}
``` 
생성자의 인자로 OwnerRepository 전달되며, 외부에서 주입이 되는 것을 알 수가 있다. (VisitRepository 마찬가지)



## IoC Container

그렇다면 스프링에서 의존성 주입은 어떤 방식으로 이루어질까?

스프링에서는 IoC 컨테이너가 알아서 의존성 주입을 해 준다.


여기서 컨테이너란?       
컨테이너는 보통 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이라 할 수 있다.

다시말해, 컨테이너란 당신이 작성한 코드의 처리과정을 위임받은 독립적인 존재이며,           
컨테이너는 적절한 설정만 되어있다면 누구의 도움없이도 `프로그래머가 작성한 코드` 를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.      

>ex. Servlet 컨테이너는 Servlet의 생성, 생성 후 초기화, 서비스 실행, 소멸에 관한 모든 권한을 가지고 있다.          
개발자들이 직접 Servlet을 생성하고 서비스하지는 않는다. 이처럼 Servlet 인스턴스에 대한 생명주기를 관리하는 기능을 가진다.      
또한, Servlet 컨테이너의 web.xml을 보면 JSP/Servlet 접근 권한에 대한 추가적인 서비스도 지원하고 있다.     
이는 Servlet의 구현과는 별도로 각 JSP/Servlet에 대한 Security를 관리해주는 기능을 한다.       



스프링이 관리하는 객체를 빈 이라고 부른다.        
(스프링 IoC 컨테이너가 관리하는 객체)      
이 IoC 컨테이너는 `프로그래머가 작성한 코드`를 참조하여, 객체를 만들어서 빈으로 등록해 놓고 이러한 빈들의 의존성들을 알아서 관리해 준다.


물론 직접 DI를 해 줄 수도 있지만,      
스프링 프레임워크가 제공하는 풍부한 DI 기능이라던가, 컨테이너의 라이프사이클 인터페이스를 통한 다양한 기능 확장이 가능하기 때문에    
IoC 컨테이너를 사용한다.

그럼 이 IoC 컨테이너에 빈으로 등록이 되려면 어떻게 해야 할까? (`프로그래머가 작성한 코드`)       
총 3가지의 방법이 있다.

1. 어노테이션(@)
``` java 
@Controller
class OwnerController {
...
```

2. 특정 인터페이스 상속
``` java 
public interface PetRepository extends Repository<Pet, Integer> {
...
```

3. 직접 빈으로 등록하는 방법
``` java 
@Configuration
class CacheConfiguration {

    @Bean
    public JCacheManagerCustomizer petclinicCacheConfigurationCustomizer() {
        return cm -> {
            cm.createCache("vets", cacheConfiguration());
        };
    }
...
```
>객체를 만들어서 리턴        
이 타입의 (JCacheManagerCustomizer) 객체가 빈으로 등록이 된다.     


       
       
참고로 인텔리J를 활용해서 볼 수도 있다.      

클래스 왼쪽에 녹색 콩 표시가 있으면 빈으로 등록된 클래스     
![빈표시](https://github.com/jaeuk2274/jaeuk2274.github.io/blob/master/_posts/img/%EC%98%88%EC%A0%9C%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%9E%85%EB%AC%B8(%EA%B0%9C%EC%A0%95%ED%8C%90)/06.Bean%20Check.png?raw=true)



IoC 컨테이너는 일반적으로 빈 팩토리(Bean Factory), 애플리케이션 컨텍스트(Application Context)를 의미한다.     

애플리케이션 컨텍스트가 빈 팩토리를 상속받기 때문에        
애플리케이션 컨텍스트 = 빈 팩토리 + a (다음 스프링 프레임워크 핵심 기술 강좌때 공부 및 추후에 다시 정리 예정)      
로 보면 된다.  


그래서 보통 스프링의 IoC 컨테이너는 일반적으로 애플리케이션 컨텍스트를 말한다.     

정리하던 중 괜찮은 설명이 있어서 가져와본다.
> 빈 팩토리를 애플리케이션 컨텍스트 또는 IoC컨테이너라 말하기도 하지만, 사실 애플리케이션 컨텍스트는 빈 팩토리를 더 확장한 개념.    
애플리케이션 컨텍스트는 그 자체로 IoC와 DI를 위한 빈 팩토리(Bean Factory)이면서 그 이상의 기능을 가짐.    
빈 팩토리와 어플리케이션 컨텍스트는 각각 BeanFactory, ApplicationContext 두 개의 인터페이스로 정의   
ApplicationContext 인터페이스는 BeanFactory 인터페이스를 상속한 서브인터페이스.    
실제로 스프링 컨테이너 또는 IoC 컨테이너라고 말하는 것은 바로 이 ApplicationContext 인터페이스를 구현한 클래스의 오브젝트.    
컨테이너가 본격적인 IoC 컨테이너로서 동작하려면 POJO클래스와 설정 메타정보가 필요.    


`IoC 컨테이너 = ApplicationContext (ApplicationContext 인터페이스를 구현한 클래스의 오브젝트)`    
즉, 스프링에서는 IoC 컨테이너가 관리하는 있는 빈들을 알아서 주입을 해준다.      

스프링 IoC 컨테이너 안에 들어있는 빈들 끼리만 주입을 해 주고,     
그 밖에 있는것은 주입을 해주지 않는다. 아주 복잡하게 가능은 하지만 쓰지 않는다고 한다.
 

직접 애플리케이션 컨텍스트를 쓸 일은 잘 없지만, 직접 선언해서 빈들이 들어있는지 확인도 가능하다.
``` java 
@Autowired
ApplicationContext applicationContext;

@Test
public void getBean(){
	applicationContext.getBeanDefinitionNames(); // 가지고 있는 빈들 전체 조회
	applicationContext.getBean(s:""); // 빈 이름 넣으면 해쉬값 반환
}

```


또한 IoC 컨테이너는 관리하는 빈(객체)를 싱글톤으로 관리한다.

직접 확인해보면,
알아서 의존성 주입 해 준 빈의 해쉬값과 애플리케이션 컨텍스트에서 꺼낸 빈의 해쉬값이 같다. (같은 인스턴스)


싱글톤 스콥(유효범위)의 객체라고 하는데,      
객체 하나를 매번 새로 만드는게 아니라 애플리케이선 전반에서 계속해서 재사용한다.     

보통 멀티쓰레드 상황에서 싱글톤 스콥을 구현하는거 자체가 조심스럽고 번거로운데      
이런 일을 IoC 컨테이너가 손쉽게 구현해준다.     
(소스코드에 특별한 코드를 넣지 않아도 IoC 컨테이너에 등록된 빈을 가져다 쓰는 식으로 굉장히 편하게 싱글톤 스콥을 구현할수 있다.)     

 
 
 
# 빈(Bean)
 
다시 빈에 대해 한번 더 정리하면,    

빈과 일반적인 객체의 차이는     
빈은 애플리케이션 컨텍스트가 관리하고 알고 있는 객체라고 보면 된다.       

직접 선언해서(new) 만든건 빈이 아니다.

오로지 빈들만 의존성 주입이 가능하다.(IoC 컨테이너)
  
아까 빈으로 등록하는 방법을 설명했지만, 좀 더 자세히 들어가자면.

## 빈을 어떻게 등록하지?

1.컴포넌트라는 메타 어노테이션을 사용하는 어노테이션 사용.      
ex. Component Scanning을 활용하기 때문 (뒤에 설명)     
	@Component        
	   - @Repository     
	   - @Service      
	   - @Controller      
	   - @Configuration      

@Controller을 들어가 확인해 보면 @Component를 확인할 수 있다.     
``` java 
@Component
public @interface Controller {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```
즉, 컨트롤러 어노테이션은 컴포넌트라는 메타 어노테이션을 사용한다.     



2.XML이나 자바 설정 파일에 등록        

XML로 선언이 가능하고, 자바로도 선언이 가능하긴 하지만 대부분 추세는 XML보다는 자바로 선언한다.     
``` java 
@Configuration
public class SampleConfig {

    @Bean
    public SampleController sampleController(){
        return new SampleController();
    }
}
``` 

@Configuration도 컴포넌트기 때문에 
컴포넌트 스캐닝이 되고
빈 설정파일이 읽힐 때 안에서 정의한 빈들이 IoC 컨테이너 안에 정의가 되기 된다.

스프링 부트의 메인에 @SpringBootApplication 
``` java 
@SpringBootApplication(proxyBeanMethods = false)
public class PetClinicApplication {

    public static void main(String[] args) {
        SpringApplication.run(PetClinicApplication.class, args);
    }
}
``` 
@SpringBootApplication 으로 들어가 보면
``` java 
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
}
``` 
@ComponentScan 이 보인다. (추후 핵심 기술에서 더 자세히 정리 예정)

 
@ComponentScan이 어디부터 어디까지 찾아라고 알려준다.(컴포넌트를 찾아보라고)       
컴포넌트 스캔이 붙어있는 위치부터 모든 하위 패키지에 있는 모든 클래스를 다 훑어보면서    
컴포넌트 어노테이션을 사용한 어노테이션들 4개(Controller..)등 이거 말고도 더 있고, 직접 정의도 가능하다.     

이 어노테이션들 붙은 것들 찾아서(컴포넌트 스캔) 빈으로 등록      
어노테이션 프로세서(처리기)가 등록이 되어 있다.      

이런 방식으로 IoC 컨테이너가 클래스를 빈으로 등록하게 된다.      



아까 2번에서 언급한 방식은 조금 다른데     
``` java 
public interface PetRepository extends Repository<Pet, Integer> {
...
```
Repository 특이한 형태로 스프링 데이터 JPA가 제공하는 기능에서 특정 인터페이스 상속반든 경우에        
이 인터페이스를 상속받고 있는 클래스를 찾아서 그 구현체를 내부적으로 만들어서 빈으로 등록한다.                
(추후 스프링 데이터 JPA에서 학습 및 정리 예정)    




## 빈을 어떻게 꺼내쓰지?

다음은 이렇게 등록한 빈을 어떠한 방식들로 꺼내 쓸수 있는가에 대해 정리해본다.

1. @Autowired 또는 @Inject
2. ApplicationContext에서 getBean()으로 직접 있다.

2번 보다는 스프링 컨테이너 자체가 주입하는 @Autowired을 대부분 사용한다.



## 필요한 의존성을 어떻게 받아올까?

빈을 꺼내는 방법은 알겠는데, 그럼 어떤 방식으로 의존성 주입을 받을 수 있을까?   
필요한 의존성을 어떻게 받아올 것이며, @Autowired / @Inject를 어디에 붙일까?


1. 생성자
2. 필드
3. Setter

3가지의 방식이 존재한다.     

1.생성자로 주입
``` java 
// 스프링 4.3부터 @Autowired 생략가능
public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
	this.owners = clinicService;
	this.visits = visits;
}
``` 
스프링 4.3 이전에는 생성자 방식도 @Autowired를 붙였어야 했다.      
하지만 4.3 이후부터는 `특정 조건`      
     
#어떤 클래스에 생성자가 하나뿐     
#주입받는 변수들이 빈으로 등록되어 있다면      
 
그 빈을 자동으로 주입해주도록 스프링 프레임웍에 기능이 추가되었기 때문에, 4.3 이상부터는 생략이 가능해졌다.     


2.필드로 주입
``` java 
@Autowired
private OwnerRepository owners;
``` 
@Autowired 만 선언해 주면 컨테이너가 알아서 의존성 주입을 해 준다. 그냥 사용하면 된다.


3.setter에 @Autowired 붙이는 방법
``` java 
private OwnerRepository owners;

@Autowired
public void setOwners(OwnerRepository owners){
   this.owners = owners;
}
```

3가지 중에서 의존성 주입 방법 중      

스프링 공식 레퍼런스에서 권장하는 방법은?     
1.생성자 방식이다.       

이 방법이 좋은 이유는     
필수적으로 생성해야 하는 레퍼런스를 만들지 못하면 생성을 강제할 수 있다.       
(이를 위한 가장 좋은 수단이 생성자를 사용하는 방법)     

OwnerController가 OwnerRepository 없으면 돌아가지 조차 않는다.(실행조차 안됨)      

그런데 필드, 세터 방식은 일단 만들수는 있다.         
근데 OwnerRepository 없는 OwnerController는 완벽한 동작을 보장하지 않는다.        
그래서 생성부터 막는것을 권장한다.        


하지만 생성자 방식은 경우에 따라 Circular Dependency(순환참조)가 발생할 수 있다.

### Circular Dependency?
순환 참조란?

A가 B에 의존하고 B가 A에 의존적이라면 어떠한 객체를 먼저 만들어야 하는지 문제가 생긴다.      
(서로를 의존할때 둘 다 생성자 인젝션을 사용하면 둘 다 못만든다..)       
서로가 서로를 의존하기 때문에 어떤 것을 먼저 만들 수 없다..     

이런 경우가 발생하였을 때,      
가장 좋은 방법은 순환 참조가 일어나지 않도록 재설계 하는 것이 가장 좋고,      
아니면 필드, 세터 방식으로 하나를 먼저 만들어 주어야 한다.     



## 과제
OwnerController에 PetRepository 주입하기.

``` java 
// final을 꼭 붙이지 않아도 상관없지만 한번 주입 받은 다음에 다른 레퍼런스로 바뀌지 않는다는 것을 보장해준다.
private final OwnerRepository owners;
private final VisitRepository visits;
private final PetRepository pets;

public OwnerController(OwnerRepository clinicService, VisitRepository visits, PetRepository pets) {
this.owners = clinicService;
this.visits = visits;
this.pets = pets;
}
``` 
 
final을 꼭 붙이지 않아도 상관없지만 한번 주입 받은 다음에 다른 레퍼런스로 바뀌지 않는다는 것을 보장해주기 위해 붙여준다.      

필트, 세터 @Autowired 구현의 경우에는 final을 붙일 수 없다.      





