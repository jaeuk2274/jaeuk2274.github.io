---
title: "스프링 데이터 JPA-01.관계형 데이터베이스와 자바, ORM" 
date: 2019-12-27
categories: 
- Back-end
tags:
- Spring 
- Data
- JPA
comments : true
---

## 관계형 데이터베이스와 자바
JDBC
- DataSource / DriverManager
- Connection
- PreparedStatement

SQL
- DDL
- DML

JDBC 사용의 문제점
- SQL을 실행하는 비용이 비싸다.(오래 걸린다)
- SQL이 데이터베이스 마다 다르다.
- 스키마를 바꿀때마다 많은 코드를 수정해야 한다.
- 반복적인 코드가 너무 많다.
- 당장은 필요가 없는데 언제 쓸 줄 모르니까 미리 다 읽어와야 하나... (필요가 없더라도 다 읽어야 한다)

JDBC 사용
~~~
try(Connection connection = DriverManager.getConnection(url, username, password)) {
    System.out.println("Connection created: " + connection);
    String sql = "INSERT INTO ACCOUNT VALUES(1, 'keesun', 'pass');";
    try(PreparedStatement statement = connection.prepareStatement(sql)) {
        statement.execute();
    }
}
~~~


도메인 모델 사용
~~~
Account account = new Account(“keesun”, “pass”);
accountRepository.save(account);
~~~

### 실습
의존성 추가 
~~~xml
<dependency>
   <groupId>org.postgresql</groupId>
   <artifactId>postgresql</artifactId>
</dependency>
~~~
PostgreSQL 설치 및 서버 실행 (docker)
~~~
docker run -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=jaeuk -e POSTGRES_DB=springdata --name postgres_boot -d postgres

docker exec -i -t postgres_boot bash

su - postgres

psql springdata

데이터베이스 조회
\list

테이블 조회
\dt

쿼리
SELECT * FROM account;
~~~


## ORM 개요

JDBC 대신 도메인 모델 사용하려는 이유
- 객체 지향 프로그래밍의 장점을 활용 가능
- 각종 디자인 패턴
- 코드 재사용
- 비즈니스 로직 구현 및 테스트 편리함.

ORM은 애플리케이션의 클래스와 SQL 데이터베이스의 테이블 사이의 맵핑 정보를 기술한 메타데이터를 사용하여, 자바 애플리케이션의 객체를 SQL 데이터베이스의 테이블에 자동으로 (또 깨끗하게) 영속화 해주는 기술.

>In a nutshell, object/relational mapping is the automated (and transparent) persistence of objects in a Java application to the tables in an SQL database, using metadata that describes the mapping between the classes of the application and the schema of the SQL database.
Java Persistence with Hibernate, Second Edition

|장점|유일한 단점|
|---|-----|
|생산성|학습비용|
|유지보수성||
|성능||
|밴더 독립성||



#### 부가적인 설명         
우리가 어떠한 한 트랜잭션 안에서 account를 저장을 했다.       
그 안의 값들을 변경할 때 만약 password를 3번 변경을 하고 저장을 한다.       
만약 sql 이라면 3번 변경이니까 update 쿼리만 3번 사용한다.         

만약 하이버네이트나 jpa같은 orm을 썼다면 실제 sql은 한번만 실행이 된다.          
객체와 데이터베이스간 캐시가 있어서 변경사항을 감지하고 이 변경사항을 반영해야 말아야 하나 결정을 한 후               
정말로 디비에 변경을 해야하는 그 시점에만 반영을 한다.     

한 트랜잭션 내에서 값을 바꿨지만 jpa가 훨씬 더 성능이 좋다.          
정말 작은 예시 중에 하나이고 성능 최적화를 위한 다양한 방법을 제공해 준다.  

또한 db를 바꾸더라도 코드는 전혀 영향이 없다.      
유일한 단점은 학습비용 발생, 생각보다 어럽다.        
하지만 장점이 훨씬 더 많음.        
특히 성능에 관련된 부분 이점이 정말 크다. 혹시라도 하이버네이트가 성능이 더 부족하다면 학습이 모자란 것.      

추천 서적          
김영한 - 자바 ORM표준 JPA프로그래밍      



## ORM 패러다임 불일치

### 객체를 릴레이션에 맵핑하려니 발생하는 문제들과 해결책

밀도(Granularity) 문제

|객체|릴레이션|
|---|-----|
|다양한 크기의 객체를 만들 수 있음.|테이블|
|커스텀한 타입 만들기 쉬움.|기본 데이터 타입 (UDT는 비추)|


서브타입(Subtype) 문제

|객체|릴레이션|
|---|-----|
|상속 구조 만들기 쉬움.|테이블 상속이라는게 없음.|
|다형성|상속 기능을 구현했다 하더라도 표준 기술이 아님.|


식별성(Identity) 문제

|객체|릴레이션|
|---|-----|
|레퍼런스 동일성 (==)|주키 (primary key)|
|인스턴스 동일성 (equals() 메소드)||


관계(Association) 문제

|객체|릴레이션|
|---|-----|
|객체 레퍼런스로 관계 표현.|외래키(foreign key)로 관계 표현.|
|근본적으로 ‘방향'이 존재 한다.|‘방향'이라는 의미가 없음. 그냥 Join으로 아무거나 묶을 수 있음.|
|다대다 관계를 가질 수 있음|태생적으로 다대다 관계를 못만들고, 조인 테이블 또는 링크 테이블을 사용해서 두개의 1대다 관계로 풀어야 함. |


데이터 네비게이션(Navigation)의 문제

|객체|릴레이션|
|---|-----|
|레퍼런스를 이용해서 다른 객체로 이동 가능.|하지만 그런 방식은 릴레이션에서 데이터를 조회하는데 있어서 매우 비효율적이다.데이터베이스에 요청을 적게 할 수록 성능이 좋다. 따라서 Join을 쓴다.|
|콜렉션을 순회할 수도 있음.|하지만, 너무 많이 한번에 가져오려고 해도 문제다.그렇다고 lazy loading을 하자니 그것도 문제다. (n+1 select)|


