## Entity Manger Factory

Entity Manger 를 생성하기 위해서는 persistence.xml의 설정 정보를 사용해서 Entity Manger Factory를 먼저 생성해야한다.

```java
EntityManagerFactory emf = Psersistence.createEntityManagerFactory("name");
```

**Entity Manager Factory 생성 비용은 굉장히 크기 때문에, 애플리케이션 전체에 걸쳐 한 번 생성하고 재사용 해야 한다.**



## Entity Manager

 Entity Manager Factory 에서 Entity Manager를 생성한다.

``` java
EntityManager em = emf.createEntityManager();
```

JPA 대부분의 기능은 Entity Manger 가 제공한다. 

따라서 Entity Manager 를 통해서 CRUD를 할 수 있기 때문에 애플리케이션 소스 입장에서는 Entity Manger를 가상의 DB라 생각할 수 있다.

**Entity Manager 는 DB connection과 밀접한 관계가 있으므로 스레드간에 공유하거나 재사용하면 안된다.**

