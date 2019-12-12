---
title: Spring Triangle-02.스프링 AOP
date: 2019-12-05
categories: 
- Back-end
- Java
tags:
- Spring
- AOP
- Proxy
- AspectJ
comments : true
---

## AOP?
AOP(Aspect-oriented programming) 관점 지향 프로그래밍이란?        
먼저 코드로 살펴보자.

```java
class A {
   method a () {
           AAAA -> AAA
           저는 퇴근후 하는 운동과 개발을 좋아해요. // 로직
           BBBB -> BB
   }
 
   method b () {
           AAAA -> AAA
           얼마 전 다이어트로 8kg를 감량했어요. // 로직
           BBBB -> BB
   }
}

class B {
  method c() {
          AAAA -> AAA
          운동도 꾸준히 하고 있습니다. // 로직
          BBBB -> BB
  }
}
```

메소드 a,b,c 전부에 실질적인 로직 앞 뒤로           
AAAA -> AAA (앞)          
BBBB -> BB  (뒤)         
가 붙어있는 것을 확인할 수 있다.               

그런데 만약 
AAAA -> AAA (앞)          
BBBB -> BB  (뒤)     
해당 소스가 수정된다면?         
AAAA -> ABC 가 되어버린다면 해당 3개의 메소드를 전부 찾아가서 변경을 해 주어야 한다..       


그럼 다음 코드를 보자.                  
``` java
class A {
   method a () {
           오늘은 7월 4일 미국 독립 기념일이래요.
   }
 
   method b () {
           저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
   }
}
 
class B {
  method c() {
          점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
  }
}

class AAAABBBB {
    method aaaabbb(JoinPoint point) {
         AAAA -> AAA 
	  point.execute() 
         BBBB -> BB
    }
}
```

이게 관점 지향 프로그래밍이다.         
공통적으로 활용되는 코드들을 분리한 것을 볼 수 있다.     

위키백과 내용을 참조하면..
>관점 지향 프로그래밍(aspect-oriented programming, AOP)은 횡단 관심사(cross-cutting concern)의 분리를 허용함으로써 모듈성을 증가시키는 것이 목적인 프로그래밍 패러다임이다.        
이를 통해 기능의 코드 핵심부를 어수선하게 채우지 않고도 비즈니스 로직에 핵심적이지 않은 동작들을 프로그램에 추가할 수 있게 한다.     

다른 참조문서 내용은..
>AOP는 관점 지향 프로그래밍으로 "기능을 핵심 비즈니스 기능과 공통 기능으로 '구분'하고, 공통 기능을 개발자의 코드 밖에서 필요한 시점에 적용하는 프로그래밍 방법"       


즉, 관심사의 분리 (핵심 비즈니스 기능과 공통 기능으로 구분)       
관심1 - 핵심 비즈니스 기능       
관심2 - 공통 기능(AAAA -> AAA / BBBB -> BB)의 분리를 허용해서(구분해서), 모듈성(해당 모듈만 수정하면 전체 수정)을 증가시키는 것        
따라서 관심1(로직)에 로직과 상관없는 것들을 분리하는 것이다.            
그래서 이런 공통 기능을 개발자의 코드 바깥에서, 필요한 시점에 적용하는 프로그래밍 방법 이라고 보면 될 것 같다.      



## AOP 구현 방법?

AOP 구현 방법에도 여러 가지가 존재하는데,     
1.컴파일 : A.java ----(AOP)---> A.class (AspectJ)        
> 내가 작성한 코드에는 없지만, 컴파일을 한 후에 컴파일된 코드에 AOP가 적용         

2.바이트코드 조작 A.java -> A.class ---(AOP)---> 메모리 (AspectJ)      
> 클래스 로더가 클래스를 읽어올 때 (=메모리에 올릴 때) 적용                   
이게 컴파일 방식이랑 뭐가 다르나면, 컴파일을 한 클래스파일에도 해당 코드가 없다.                 
그런데 클래스를 로딩하는 그 시점. 그때 메모리상 그 메서드에는 해당 코드가 들어가게 되는 것이다.                  
클래스 로더에 특별한 옵션이 있어서 (로딩 시점 바이트코드 조작) 그런 구현이 가능하다고 한다.        
(추후 클래스 로더에 대해 더 자세히 다룰 예정)           

3.프록시 패턴 (스프링 AOP가 사용하는 방법)     

스프링에서는 디자인 패턴 중 하나인 프록시 패턴을 사용하여, AOP와 같은 동작을 구현한다.             

### proxy pattern?     
프록시 패턴 참조 문서 : https://refactoring.guru/design-patterns/proxy       

먼저 코드로 프록시 패턴을 설명하자면        
Store
``` java
public class Store {

    Payment payment;

    public Store(Payment payment){
        this.payment = payment;
    }

    public void buySomething(int amount) {
        payment.pay(amount);
    }
}
```
``` java
public interface Payment {

    void pay(int amount);
}
```

``` java
public class Cash implements Payment {
    @Override
    public void pay(int amount){
        System.out.println(amount + "현금 결제");
    }
}
```
``` java
public class CashPerf implements Payment{

    Payment cash = new Cash();

    @Override
    public void pay(int amount) {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        cash.pay(amount);

        stopWatch.stop();
        System.out.println(stopWatch.prettyPrint());
    }
}
```
``` java
public class StoreTest {

    @Test
    public void testPay(){
        // Payment cash = new Cash(); // 기존
        Payment cash = new CashPerf(); // 성능 측정 추가
        Store store = new Store(cash);
        store.buySomething(100);
    }
}
```
```
100현금 결제
StopWatch '': running time = 109019 ns
---------------------------------------------
ns         %     Task name
---------------------------------------------
000109019  100%  
```


기존 코드의 변경 없이 성능 측정 기능 추가

차근차근 살펴보면,        
Cash와 CashPerf가 구현하는 Payment 인터페이스를 만들었다.      
따라서 이 두 클래스는 Payment 타입이다.      
Store을 생성할때도 Payment 타입으로 생성한다.       
>public Store(Payment payment){        
또한        
CashPerf는 똑같은 메서드를 오버라이딩해서 Cash메서드를 그대로 사용하되, 앞 뒤로 성능을 측정하는 기능을 넣은 것을 볼 수 있다.(StopWatch) 
이런 방식이 프록시 패턴     
프록시(대리인-CashPerf)을 두고 그 대리인이 Cash를 사용하는 것이다.   
새로운 코드를 추가하긴 했지만 기존 코드를 건들지도, 영향도 없는 것을 확인할 수 있다.        
(클라이언트의 영향을 주지 않으면서 객체를 다른 객체로 바꾸는 방법)       


## 스프링 AOP

이런 방식이 AOP를 프록시 패턴으로 구현하는 방법이다.        
스프링 AOP에서는 복잡한 내부 매커니즘을 통해 이런 방식이 자동으로 구현되어진다.        


### 복잡한 내부 매커니즘(간략 설명) 
> 원래는 Cash가 빈으로 등록이 되어야 하는데,       
Cash 대신에 CashPerf(프록시)가 빈으로 등록되고,       
그래서 이 클라이언트가 원래 빈으로 등록되어야 하는 Cash가 아닌 CashPerf를 대신 쓰게 된다.    
이런 일이 스프링 내부에서 발생한다.       

####  @Transctional
트랜잭션 어노테이션도 스프링 AOP 기반으로 만들어진 것이며, 스프링 AOP가 사용하는 어노테이션이다.             

```
public interface OwnerRepository extends Repository<Owner, Integer> {

    @Query("SELECT DISTINCT owner FROM Owner owner left join fetch owner.pets WHERE owner.lastName LIKE :lastName%")
    @Transactional(readOnly = true)
    Collection<Owner> findByLastName(@Param("lastName") String lastName);
```  

트랜잭션 어노테이션이 붙어있으면      
ex. @Transactional(readOnly = true)     
트랜잭션이 앞뒤로 붙어있는 프록시로 객체 타입이 만들어 진다.      
ex. 코드가 앞뒤로 붙는다.(오토커밋 false, sql 실행 후 롤백 롤백 혹은 커밋 등..)       

이런 코드를 생략할 수 있게 트랜잭션 어노테이션이 그 코드를 앞뒤로 넣어주는 것이다.      

앞서 구현한 스톱와치 프록시 패턴 방식으로 구현된 것과 똑같은 방식이며,
개발자는 비즈니스 로직에만 집중할 수 있게 도와준다.  

 
### 스프링 AOP 구현

```
@GetMapping("/owners/{ownerId}/edit")
@LogExecutionTime
public String initUpdateOwnerForm(@PathVariable("ownerId") int ownerId, Model model) {
	Owner owner = this.owners.findById(ownerId);
	model.addAttribute(owner);
	return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
}
```

```
@Target(ElementType.METHOD) // 이 어노테이션을 어디에 사용할건지
@Retention(RetentionPolicy.RUNTIME) // 이 어노테이션 정보를 언제까지 유지할건지
public @interface LogExecutionTime {
}
```

```
@Component
@Aspect
public class LogAspect {
    Logger logger = LoggerFactory.getLogger(LogAspect.class);
    
    @Around("@annotation(LogExecutionTime)") // Around 어노테이션 사용하면 joinPoint 파라미터 받을 수 있음. - 해당 메서드
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        Object proceed = joinPoint.proceed();

        stopWatch.stop();
        logger.info(stopWatch.prettyPrint());

        return proceed;
    }
}
```

----
@LogExecutionTime 어노테이션을 (어디에 사용할지, 언제까지 유지할건지 등) 만들고,       
LogAspect 클래스에서 @Around("@annotation(LogExecutionTime)")로 받아 성능 측정을 하는 logExecutionTime를 구현하였다.
이후 사용은 initUpdateOwnerForm 메서드에서 @LogExecutionTime 으로 사용하는 것을 볼 수 있다.



+추가적으로 인텔리j 에서 이 Aspect가 어디에 사용이 되고 있는지 툴의 도움을 받아 한번에 볼 수도 있다.          
![aspect check](https://github.com/jaeuk2274/jaeuk2274.github.io/blob/master/_posts/img/%EC%98%88%EC%A0%9C%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%9E%85%EB%AC%B8(%EA%B0%9C%EC%A0%95%ED%8C%90)/05.Aspcet%20use%20all%20check.png?raw=true)
