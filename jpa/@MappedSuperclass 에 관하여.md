## @MappedSuperclass 에 관하여

 일반적으로, 상속 관계 매핑 전략에서 부모 클래스와 자식 클래스 모두 데이타베이스 테이블과 매핑을 한다. 이와 달리, **부모 클래스를 상속받는 자식클래스에게 매핑 정보만 제공하고 싶을때** 이 어노테이션을 사용하면 된다.

 엔티티 종류에 상관없이 공통으로 가지고 있어야 하는 정보가 있다면 ( ex. 데이타 생성시간, 수정시간 등 ) 공통 클래스로 추출하고 이를 상속받는 방식으로 구현할 때 사용 한다. 그러나 엔티티는 엔티티만 상속받을 수 있기 때문에 엔티티가 아닌 클래스를 상속받기 위해서 @MappedSuperclass 를 사용한다



## 예제

 아래 2개의 엔티티를 보자.

```java
@Data
@Entity
public class Car extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long carId;

    private String carName;

}
```



```java
@Data
@Entity
public class Book extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long bookId;

    private String bookName;

}
```



데이타베이스에는 Car와 Book, 그 외 언급되지 않은 수많은 테이블이 있다고 하자. 우리는 데이타 관리를 위해 모든 데이타에 **생성일시와 수정일시 column을** 가지게 하려고 한다. 



```java
@MappedSuperclass
public class BaseEntity {

    private Date createdAt;

    private Date updatedAt;

}
```



  이때 BaseEntity 라는 class를 위와 같이 정의하고 @MappedSuperclass 어노테이션을 정의 해준다. 그리고 각 엔티티들은 BaseEntity를 상속받는다. 그렇게 되면 각 테이블의 createdAt, updatedAt colmun과 매핑 되게 된다.



## @AttributeOverride

만약 book 테이블의 생성일시만 createdAt이 아닌 publishedAt 으로 바꾸고 싶으면 Book만 BaseEntity를 상속받지 않고 따로 만들어야할까 ?

그럴 필요는 없고, 아래와 같이 **@AttributedOverride** 어노테이션으로 필요한 매핑정보만 재정의가 가능하다.

```java
@Data
@Entity
@AttributeOverride(name = "createdAt", column = @Column(name = "publishedAt"))
public class Book extends BaseEntity { ...
```

또한,

```java
@AttributeOverrides({
        @AttributeOverride(...),
        @AttributeOverride(...)
})
```

위와 같은 형식으로 여러개의 매핑정보도 한번에 재정의 할 수 있다.



## 정리

@MappdedSupperclass 는

> - 테이블과 매핑되지 않고 자식 클래스 엔티티의 매핑정보를 상속하기 위해 사용된다.
> - @MappedSupperclass로 지정한 클래스는 엔티티가 아니다 -> 즉, entityManager.find()나 JPQL 을 사용할 수 없다.
> - 이 클래스만 단독으로 사용할일은 거의 없기 때문에, 방어코드 차원에서 abstract 로 선언하여 사용하는 것이 실수를 줄일 수 있다.

