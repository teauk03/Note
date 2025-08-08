# 영속성 컨텍스트
* JPA와 Hibernate 같은 구현체에서 엔티티 인스턴스를 관리하는 메모리 저장소
*  영속성 컨텍스트라는 저장소는 [EntityManager](https://github.com/teauk03/Note/blob/main/jpa/Entity%20Manager%2CEntity%20Manager%20Factory%20md)가 관리하며, 엔티티의 생성, 조회, 수정, 삭제 등 전 과정을 담당한다.가 관리하며, 엔티티의 생성, 조회, 수정, 삭제 등 전 과정을 담당한다.
* 엔티티의 상태 변화를 추적하여, 변경 감지(Dirty Checking) 등 핵심 기능을 제공한다.
* `EntityManager`를 하나 생성할 때 `영속성 컨텍스트` 하나가 만들어짐


## 1. 영속성 컨텍스트의 기본 제약사항

### 1.1. 식별자 값은 필수 
```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Entity
@Table(name = "users")
@Getter
@NoArgsConstructor
public class User {

    @Id // ⭐️ 이게 필수라는 말
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String email;

    @Column(nullable = false)
    private String password;

    public User(String email, String password) {
        this.email = email;
        this.password = password;
    }
}
```

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT, // ⭐️
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);
```
* 영속 상태로 존재하는 엔티티는 반드시 `식별자 값(@ID)`이 있어야 함
    * 영속성 컨텍스트는 <U>엔티티 객체를 식별자 값으로 구분</U>하기 때문
* 식별자 값이 없으면 동일한 엔티티인지 판별할 수 없어, 1차 캐시(영속성 컨텍스트 내부 저장소)에 올릴 수 없어 예외가 터짐

### 1.2. 동일 식별자를 가진 두 객체는 동시에 영속 상태가 될 수 없음
```java
new User(1L, "a@a.com", "1234")
new User(1L, "b@b.com", "5678")
// → 둘 다 같은 id 값을 가지고 persist()하면 예외 발생 (동일성 보장 위배)
```

### 1.3. 영속성 컨텍스트는 엔티티의 주소값 기준 동일성을 보장
```java
User user1 = em.find(User.class, 1L);
User user2 = em.find(User.class, 1L);
System.out.println(user1 == user2); // true
```

### 1.4. flush 시점 전에 commit하지 않으면 DB에 반영되지 않음
* 영속성 컨텍스트에 저장된 변경 사항은 트랜잭션 커밋 전까지 DB에 반영되지 않음
* `flush()` or `commit()`이 되어야 SQL이 실행됨

### 1.5. 변경 감지(Dirty Checking) 작동 조건
* 트랜잭션 범위 내에서 영속 상태인 객체의 필드를 setter 등으로 변경해야 감지됨
* 단순히 객체를 바꾸거나 detached 상태에서 변경하면 감지되지 않음

<br></br>

## 2. 영속성 컨텍스트의 동작
![alt text](<설명사진/[JPA - 영속성 컨텍스트] 영속성 컨텍스트의 전체적인 동작 과정 .png>)
```java
// 엔티티 매니저 팩토리 생성
EntityManagerFactory emf = Persistence.createEntityManagerFactory("myJpaUnit");
// 엔티티 매니저 생성
EntityManager em = emf.createEntityManager();
// 해당 매니저의 트랜잭션 생성 (tx)
EntityTransaction tx = em.getTransaction();

try {
    tx.begin(); // 트랜잭션 시작

    // 1. 비영속 상태 객체 생성
    User user = new User("1234", "user");

    // 2. 영속성 컨텍스트에 등록 
    em.persist(user);

    // 3. flush + commit → 실제 DB에 작업 수행
    em.flush();
    tx.commit(); // 트랜잭션 커밋

} catch (Exception e) {
    tx.rollback(); // 예외 시 롤백
} finally {
    em.close();     // EntityManager 종료
    emf.close();    // EntityManagerFactory 종료
}
```

### 2.1. 비영속 엔티티 → 컨텍스트에 영속
```java
// 객체 생성
User user = new User("1234", "user);
```
* 객체를 생성한 상태로 영속성 컨텍스트와 관련이 없음 (DB에 어떤 영향도 줄 수 없는 상태)
```java
em.persist(user);
```
* `persist()` 를 호출하여 user 객체를 영속성 컨텍스트에 등록
![alt text](<설명사진/[JPA - 영속성 컨텍스트] 영속성 컨텍스트의 동작 과정 (1)png.png>)

* 영속 상태가 된 user 객체는 영속성 컨텍스트 안에 있는 1차 캐시에 등록
![alt text](<설명사진/[JPA - 영속성 컨텍스트] 영속성 컨텍스트의 동작 과정 (2)png.png>)
    * 영속 상태인 엔티티 객체는 모두 이 연속성 컨텍스트의 1차 캐시에 등록됨
    * 1차 캐시는 `Map 형태`로 저장되며 `key 값은 @Id`로 식별한 식별자, `value는 엔티티 객체`임

### 2.2. 1차 캐시에서 조회
```java
// 1차 캐시에서 조회
User findUser = em.find(User.class, "user);
```
* 영속성 컨텍스트의 1차 캐시에서 삭별자 값(@ID)을 가진 객체를 찾음
    * 만약 해당 객체가 있다면 즉시 반환함
    ![alt text](<설명사진/[JPA - 영속성 컨텍스트] 영속성 컨텍스트의 동작 과정 (3).png>)
    * 만약 해당 객체가 없다면 DB에서 조회하고 결과를 영속성 컨텍스트에 저장하고 반환함
    ![alt text](<설명사진/[JPA - 영속성 컨텍스트] 영속성 컨텍스트의 동작 과정 (4).png>)

### 2.3. 엔티티 수정
![alt text](<설명사진/[JPA - 영속성 컨텍스트] 영속성 컨텍스트의 동작 과정 (5) - 변경 감지.png>)
* 영속성 컨텍스트는 트랜잭션이 시작되면 엔티티의 초기 상태를 스냅샷으로 저장함
* **Dirty Checking(변경 감지):** 트랜잭션이 진행되는 동안 엔티티의 값이 변경되면, 스냅샷과 현재 상태를 비교하여 변경된 필드만 찾아내고, 그 차이에 해당하는 `UPDATE 쿼리`를 자동으로 생성함
```java
// 스냅샷으로 찍어두었던 엔티티 초기의 상태
    User user = new User("1234", "user");

// 뱐경 감지 
    user.setUserName("admin");

```


### 2.4. 쓰기 지연 SQL 저장소
* JPA는 **쓰기 지연 전략** 을 사용함
* `em.persist()`, `em.remove()`, `Dirty Checking` 등으로 인해 발생한 SQL들은 **즉시 실행되지 않고 저장소에 누적**됨
- 이 저장소는 **영속성 컨텍스트 내부**에 존재하며, `flush()` 또는 `commit()` 시점에 실행됨

```java
em.persist(user); // INSERT 지연
user.setUserName("admin"); // Dirty Checking
```


### 2.5. flush
```java
em.flush();
```
* 영속성 컨텍스트의 쓰기 지연 저장소에 있던 SQL들이 DB로 전송됨
* 하지만 트랜잭션은 여전히 열린 상태이며, rollback()도 가능함
* `commit()` 시점에 자동으로 호출 됨
* `JPQL 쿼리 실행` 시 자동으로 호출돔


### 2.6. commit 
```java
tx.commit();
```
* 트랜잭션 커밋 시점에 `flush()`가 자동 호출되어 DB에 변경사항이 반영됨
* 이후 영속성 컨텍스트는 종료되고, 엔티티는 준영속 상태로 전환됨