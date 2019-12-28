---
title: "스프링 데이터 JPA-03.관계 매핑, 엔티티 상태와 Cascade"
date: 2019-12-28
categories: 
- Back-end
tags:
- Spring 
- Data
- JPA
comments : true
---

## JPA 프로그래밍 4. 관계 매핑

관계에는 항상 두 엔티티가 존재
- 둘 중 하나는 그 관계의 주인(owning)이고
- 다른 쪽은 종속된(non-owning) 쪽입니다.
- 해당 관계의 반대쪽 레퍼런스를 가지고 있는 쪽이 주인.

단방향에서의 관계의 주인은 명확합니다.
- 관계를 정의한 쪽이 그 관계의 주인입니다.

단방향 @ManyToOne
- 기본값은 FK 생성

단방향 @OneToMany
- 기본값은 조인 테이블 생성

양방향
- FK 가지고 있는 쪽이 오너 따라서 기본값은 @ManyToOne 가지고 있는 쪽이 주인.
- 주인이 아닌쪽(@OneToMany쪽)에서 mappedBy 사용해서 관계를 맺고 있는 필드를 설정해야 합니다.

양방향
- @ManyToOne (이쪽이 주인)
- @OneToMany(mappedBy)
- 주인한테 관계를 설정해야 DB에 반영이 됩니다.

### 실습

~~~java
@Entity //(name = "myAccount") // 미설정시 클래스 이름, 객체 안에서 사용되는 이름
//@Table(name = "account") // 미설정시 엔티티 네임과 동일
public class Account {

    @Id
    @GeneratedValue // 자동으로 생성되는 값, 리포지터리 통해 저장할때 자동으로 생성된 값을 씀
    private Long id;

    @Column(nullable = false)
    private String username;

    private String password;

    @OneToMany(mappedBy = "owner")
    private Set<Study> studies = new HashSet<>();
~~~


~~~java
@Entity
public class Study {
    @Id @GeneratedValue
    private long id;

    private String name;

    @ManyToOne
    private Account owner;
~~~


~~~java
@Component
@Transactional
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext // jpa 가장 핵심적인 빈
    EntityManager entityManager;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account account = new Account();
        account.setUsername("jaeuk");
        account.setPassword("jpa");

        Study study = new Study();
        study.setName("Spring Data JPA");

        // 01. 이 둘은 한 세트(객체지향적으로 보았을 때)
        //account.getStudies().add(study);
        //study.setOwner(account); // 이 줄만 있어도 db insert는 된다.

        // 02. 그래서 일반적으로 이렇게 사용한다.
        account.addStudy(study);

        Session session = entityManager.unwrap(Session.class);
        session.save(account);
        session.save(study);
    }
}
~~~

Account
~~~java
    // 일반적으로 이렇게 사용한다.
    public void addStudy(Study study) {
        this.getStudies().add(study);
        study.setOwner(this);
    }

    // remove도 마찬가지
    public void removeStudy(Study study) {
        this.getStudies().remove(study);
        study.setOwner(null);
    }
~~~
