![alt text](<설명사진/[JPA] 엔티티 매니저 팩토리와 엔티티 매니저.png>)
# 1. Entity Manager Factory
* EntityManager 객체를 셍성하는 공장
* 하나의 애플리케이션에서 하나만 생성함 
* `EntityManagerFactory`는 <U>초기 생성 시 DB 메타데이터를 분석하고 설정 정보를 로딩하기 때문에 무겁고 비용이 많이 드는 작업</U>임
* 때문에 JPA에선 한 개만 만들어서 애플리케이션 전체에 공유하도록 설계 되었음
* 서로 다른 여러 가지의 스레드 간에 공유가 가능함 (스레드 안정성)

```java
// 엔티티 매니저 팩토리 생성 
EntityManagerFactory emf = Persistence.createEntityManagerFactory("my-persistence-unit");
```

<br></br>


# 2. Entity Manager
* JPA에서 실제로 DB에 관련된 작업을 담당하는 객체
* 엔티티 CRUD 작업을 담당함
* 내부적으로 `영속성 컨텍스트`를 관리 함
* 스레드에 안전하지 않기 때문에, 각 요청이나 트랜잭션마다 별도의 인스턴스를 생성해서 사용해야 함

```java
EntityManager em = emf.createEntityManager();

em.getTransaction().begin(); // 트랜잭션 시작

// 데이터베이스 작업

em.getTransaction().commit(); // 트랜잭션 커밋

em.close(); // 엔티티 매니저 종료
```