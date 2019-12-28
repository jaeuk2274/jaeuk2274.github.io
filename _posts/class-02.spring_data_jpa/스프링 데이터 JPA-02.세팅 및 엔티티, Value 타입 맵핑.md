---
title: "스프링 데이터 JPA-02.세팅 및 엔티티, Value 타입 맵핑"
date: 2019-12-27
categories: 
- Back-end
tags:
- Spring 
- Data
- JPA
comments : true
---

## JPA 프로그래밍 1. 프로젝트 세팅

데이터베이스 실행
- PostgreSQL 도커 컨테이너 재사용
- docker start postgres_boot

스프링 부트
- 스프링 부트 v2.*
- 스프링 프레임워크 v5.*

스프링 부트 스타터 JPA
- JPA 프로그래밍에 필요한 의존성 추가
  - JPA v2.*
  - Hibernate v5.*
- 자동 설정: HibernateJpaAutoConfiguration
  - 컨테이너가 관리하는 EntityManager (프록시) 빈 설정
  - PlatformTransactionManager 빈 설정


JDBC 설정
~~~
spring.datasource.url=jdbc:postgresql://localhost:5432/springdata
spring.datasource.username=jaeuk
spring.datasource.password=pass

# drop - create
spring.jpa.hibernate.ddl-auto=create
# sql show
spring.jpa.show-sql=true
# sql show할 때 포멧 (보 기편하게)
spring.jpa.properties.hibernate.format_sql=true
#warning 제거
spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true 
~~~


## JPA 프로그래밍 2. 엔티티 맵핑

#### @Entity
- “엔티티”는 객체 세상에서 부르는 이름.        
- 보통 클래스와 같은 이름을 사용하기 때문에 값을 변경하지 않음.     
- 엔티티의 이름은 JQL에서 쓰임.       

#### @Table
- “릴레이션" 세상에서 부르는 이름.      
- @Entity의 이름이 기본값.      
- 테이블의 이름은 SQL에서 쓰임.         

#### @Id
- 엔티티의 주키를 맵핑할 때 사용.        
- 자바의 모든 primitive 타입과 그 랩퍼 타입을 사용할 수 있음      
- Date랑 BigDecimal, BigInteger도 사용 가능.      
- 복합키를 만드는 맵핑하는 방법도 존재(추후 다룸)               

#### @GeneratedValue
- 주키의 생성 방법을 맵핑하는 애노테이션       
- 생성 전략과 생성기를 설정할 수 있다.       
- 기본 전략은 AUTO: 사용하는 DB에 따라 적절한 전략 선택      
- TABLE, SEQUENCE, IDENTITY 중 하나.          

#### @Column
- unique
- nullable
- length
- columnDefinition
- ...

#### @Temporal
- 현재 JPA 2.1까지는 Date와 Calendar만 지원.
- 이후부터 자바8에서 등장한 새로운 시간타입 지원

#### @Transient
- 컬럼으로 맵핑하고 싶지 않은 멤버 변수에 사용. (객체에서만 사용)



## JPA 프로그래밍 3. Value 타입 맵핑

#### Value 타입 종류
- 기본 타입 (String, Date, Boolean, ...)
- Composite Value 타입
- Collection Value 타입
- 기본 타입의 콜렉션
- 컴포짓 타입의 콜렉션

#### Composite Value 타입 맵핑
- @Embeddable
- @Embedded
- @AttributeOverrides
- @AttributeOverride

예시 
~~~java
@Embeddable
public class Address {

    private String street;

    private String city;
~~~

~~~java
@Entity //(name = "myAccount") // 미설정시 클래스 이름, 객체 안에서 사용되는 이름
//@Table(name = "account") // 미설정시 엔티티 네임과 동일
public class Account {

    @Id
    @GeneratedValue // 자동으로 생성되는 값, 리포지터리 통해 저장할때 자동으로 생성된 값을 씀
    private Long id;

    private String username;

    private String password;

    @Embedded
    @AttributeOverrides({
            @AttributeOverride(name = "street", column = @Column(name = "home_street") ),
            @AttributeOverride(name = "city", column = @Column(name = "home_city") )
    })
    private Address homeAddress;
...
~~~

~~~
    create table account (
       id int8 not null,
        active boolean not null,
        home_city varchar(255), 
        home_street varchar(255),
        city varchar(255),
        street varchar(255),
        password varchar(255),
        username varchar(255),
        primary key (id)
    )
~~~
