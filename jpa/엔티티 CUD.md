### 엔티티 등록



엔티티 매니저를 이용해서 엔티티를 영속성 컨텍스트에 등록한다

```java
1	transaction.begin();
2	em.persist(memA);
3	em.persist(memB);
4	transaction.commit();
```

트랜잭션이 시작된 후 memA 를 persist 하면 DB에 바로 반영되는 것이 아니다. memA의 Insert Query 를 영속성 컨텍스트 내의 내부 쿼리 저장소(쓰기 지연 SQL 저장소)에 저장하고 1차 캐시에만 엔티티를 저장한다. memB를 persist 할 때에도 마찬가지이다.

 마지막으로 `transaction.commit()`이 호출될 때 flush가 일어나고 이때 실제 DB로 SQL을 날린다.

persist 할 때마다 쿼리를 날리든, 모아서 한번에 날리든 **결국 트랜잭션 커밋이 일어나야 최종적으로 DB에 반영하는 것**이 맞기 때문에 이와 같은 방법이 가능하다



### 엔티티 수정



#### Dirty check (변경 감지)

 JPA는 엔티티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 스냅샷을 뜬다. 그리고 플러시 시점에 스냅샷과 엔티티를 비교하여 Dirty Check 를 한다. 변경된 엔티티가 있을 시 Update Query를 내부 쿼리 저장소에 저장한다. 이때 쿼리는 특정 변경 값이 아닌 모든 필드를 SET한다.(SQL 재사용 용이)

**이는 Managed 상태인 엔티티에만 적용 된다.**

#### DynamicUpdate

`@org.hibernate.annotationsDynamicUpdate`  애노테이션을 통하여 특정 변경 값에 대한 Update Query를 생성하도록 지정할 수 도 있다. 일반적으로 컬럼이 대략 30개 이상이 되면 기본 방법보다 빠르다고 한다.



### 엔티티 삭제

```java
entityManager.remove(memA);
```

삭제도 마찬가지로 즉시 수행되는 것이 아니라 삭제 쿼리를 내부 쿼리 저장소에 저장한 후, 트랜잭션 커밋이 일어날 때 삭제쿼리를 DB로 전달한다. 

하지만 remove가 호출되는 순간 바로 memA는 영속성 컨텍스트에서 제거된다.