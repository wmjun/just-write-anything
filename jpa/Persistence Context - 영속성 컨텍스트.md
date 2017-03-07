## Persistence Context - 영속성 컨텍스트

영속성 컨텍스트는 엔티티 매니저를 생성할 때 같이 만들어 지며 엔티티 매니저를 통해서 영속성 컨텍스트에 접근할 수 있다. 즉, 엔티티 매니저가 하는 어떤 행위는 영속성 컨텍스트에 반영되고 이것이 최종적으로 DB에 반영된다.

### 특징

- 식별자 값
  - 영속성 컨텍스트는 엔티티를 식별자 값으로 구분한다(@Id 애노테이션)
- 데이터베이스와의 관계
  - 영속성 컨텍스트에 엔티티가 저장된다고 바로 DB에 반영되는 것이 아니라 일반적으로 트랜잭션이 커밋될 때 DB에 반영된다.(flush)
- 영속성 컨텍스트에 엔티티를 관리할 때의 장점
  - 1차캐시
  - 동일성 보장
  - 트랜잭션이 지원되는 지연 쓰기
  - 변경 감지
  - 지연 로딩



###  Entity Life Cycle

- new / transient : 비영속 상태라고 하며 영속성 컨텍스트와 관계가 없는 상태이다.
- managed : 영속 상태. 영속성 컨텍스테 저장된 상태이며 엔티티 매니저에 의해 관리되고 있는 상태이다.
- detached : 준영속 상태. 영속성 컨텍스테 저장되었다가 분리된 상태.
- removed : 삭제된 상태.



### 엔티티를 조회하면 ?

영속성 컨텍스트는 내부 캐시를 가지고 있는데 이를 **1차 캐시**라고 한다. 영속 상태의 엔티티는 모두 이곳에 저장 된다. 1차 캐시는 Entity의 @Id를 Key로 하고 인스턴스를 Value로 하는 Map의 형태를 띈다.

```java
Member member1 = entityManager.find(Member.class, "member1");
```

위와 같이 member1을 조회할 때. 먼저 영속성 컨텍스트의 1차 캐시에서 먼저 찾는다. 만약 1차 캐시에 엔티티가 존재하지 않으면 DB를 조회하여 가져오며, 이때 1차 캐시에도 같이 저장한다.



### 동일성 보장

```java
Member memberA = entityManager.find(Member.class, "member1");
Member memberB = entityManager.find(Member.class, "member1");

System.out.println(memberA == MemberB);
```

어떻게 출력이 될까? 당연히 True 이다.  `find(Member.class, "member1")` 를 반복해서 호출해도 1차 캐시에 있는 같은 엔티티 인스턴스를 반환하기 때문이다.



