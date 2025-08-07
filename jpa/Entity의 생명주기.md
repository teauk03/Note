# Entity의 생명주기
![alt text](<설명사진/[JPA] 엔티티 객체의 상태.png>)
* JPA에서 `엔티티(Entity)`객체는 영속성 컨텍스트와의 관계에 따라 다음 4가지 생명주기 상태를 가짐

## 1. 비영속 상태 (new)
![alt text](<설명사진/[JPA] 엔티티 객체의 상태 - (1) new state.png>)
```java
User user = new User();
user.setId("user1");
user.setEmail("user@example.com");
```
* 영속성 컨텍스트와 전혀 관계가 없는 상태를 의미
* 엔티티 객체를 생성은 하였으나 아직 영속성 컨텍스트에 저장하지 않은 상태 

## 2. 영속 상태 (managed)
![alt text](<설명사진/[JPA] 엔티티 객체의 상태 - (2) managed state.png>)
```java
em.persist(user);
```
* `Entity Manager`를 통해 엔티티 객체를 영속성 컨텍스트에 저장되어 이 컨텍스트의 관리 대상이 된 상태
* `.find()` 메서드나 `JPQL`로 조회한 객체도 영속성 컨텍스트가 관리하는 대상임

## 3. 준영속 상태 (detached)
![alt text](<설명사진/[JPA] 엔티티 객체의 상태 - (3) detched state.png>)
```java
em.detach(user);
em.clear();
em.close();
```
* 영속성 컨텍스트가 관리하던 객체가 관리대상에서 제외된 상태
* 영속성 컨텍스트를 닫거나, 초기화하는 경우 준영속 상태에 포함됨

## 4. 삭제 상태 (removed)
```java
em.remove(user);
```
* 엔티티를 영구적으로 영속성 컨텍스트와 데이터 베이스에서 삭제함
* 여기서 객체는 GC의 수거 대상인지 아닌지에 따라 살아 있을 수도 있음