## JPA 기본 키 전략



### 1. 직접 할당

`@id` 애노테이션으로 필드와 매핑한다. 자바 기본형, 래퍼형, String, Date, BigDecimal, BigInteger가 타입이 가능하다



### 2. IDENTITY

기본 키 생성을 데이타베이스에 위임한다. 데이타베이스의 auto_increment와 같은 기능을 사용할 때 쓴다. 키 필드에 `@GeneratedValue(strategy = GenerationType.IDENTITY)`를 사용한다.

이 전략을 사용하면 JPA는 기본 키 값을 얻어오기 위해 데이타베이스를 추가로 조회한다. 따라서 이 전략을 사용하는 엔티티를 새로 생성하여 식별자 값을 할당하려면 1차 캐시를 넘어서 데이타베이스에서 Insert한 후에 기본 키 값을 조회한다. 즉, `persist()`를 호출하는 즉시 Insert SQL이 데이타베이스에 전달되므로 트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.



### 3. SEQUENCE

>  데이타베이스 시퀀스 :  유일한 값을 순서대로 생성하는 데이타베이스 오브젝트	

시퀀스를 사용해서 기본 키를 생성하는 전략이다. 오라클, PostgreSQL, DB2, H2 등이 시퀀스를 지원한다.

```java
@Entity
@SequenceGenerator(name = "MY_SEQ_GENERATOR", sequnceName = "MY_SEQ"
                  initialValue = 1, allocationSize = 1)
public class Board {
  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "MY_SEQ_GENERATOR")
  private Long id;
}
```

`@SequenceGenerator`를 의 속성에서 name에 식별자 생성기 이름을 정해준다. 그리고 sequenceName에 DB에 등록되어 있는 시퀀스 이름을 지정해주고, 초기값(initialValue)과 한번 호출에 증가하는 수(allocationSize)를 입력하여 시퀀스 생성기를 등록한다. 



이 전략에서 `persist()`를 호출할 때 DB의 시퀀스를 사용하여 식별자를 조회한다. 그리고 그 식별자를 엔티티에 할당한 후 엔티티를 영속성 컨텍스트에 저장한다. 



### 4. TABLE

키 생성 전용 테이블을 하나 만들고, 여기에 이름과 값으로 사용할 컬럼을 만들어 데이타베이스의 시퀀스처럼 동작하게 하는 전략.

```java
@Entity
@TableGenerator(name = "MY_SEQ_GENERATOR", table = "MY_SEQUNCES"
                   pkColumnValue = "MY_SEQ", allocationSize = 1)
public class Board {
  @Id
  @GeneratedValue(strategy = GenerationType.TABLE, generator = "MY_SEQ_GENERATOR")
  private Long id;
}
```



MY_SEQUNCES 테이블은 아래와 같을 것이다

> ***sequnceName***		***next_val***
>
> MY_SEQ				2

`@TableGenerator.pkColumnValue`에서 지정한 "MY_SEQ"가 컬럼명으로 추가되었고, 키 생성기를 사용하여 기본 키를 할당할 때마다 next_val 컬럼 값이 증가한다.