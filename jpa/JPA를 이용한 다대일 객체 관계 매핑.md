## JPA를 이용한 다대일 객체 관계 매핑 

한 회원은 하나의 팀에 속하며, 한 팀에는 여러 회원이 속하는 경우를 생각해보자. 

팀은 회원에 대한 정보를 몰라도 괜찮다고 하면 아래와 같이 표현할 수 있다.


#### JPA Entity

**Member.java**

```java
@Entity
public class Memeber {
  @Id
  @GeneratedValue
  @Column(name = "memberId")
  private Long id;
  
  private String userName;
  
  @ManyToOne
  @JoinColumn(name = "teamId")
  private Team team

  //bla bla
}
```

`@JoinColumn`을 사용하여 Team과 다대일 관계를 매핑한다. 

**@JoinColumn 주요 속성**

- `name` - 속성은 외래키 컬럼의 이름을 지정한다. 기본 값은 `필드명_참조하는 테이블의 PK`로 정해진다.


- `referencedColumnName`  - 외래 키가 참조하는 테이블의 컬럼명



**Team.java**

```java
@Entity
public class Team {
  @Id
  @GeneratedValue
  @Column(name = "teamId")
  private Long id;
  
  private String teamName;
  
  //bla bla
}
```



팀에서는 회원에 대한 정보를 몰라도 되기 때문에 매핑을 하지 않았다.

만약 팀에서도 회원에 대한 정보가 필요하여 양방향 매핑이 필요하다면 다음과 같이 하면 된다.

**Team.java**

```java
@Entity
public class Team {
  @Id
  @GeneratedValue
  @Column(name = "teamId")
  private Long id;
  
  private String teamName;
  
  @OneToMany(mappedBy="team")
  private List<Member> members;
  
  //bla bla
}
```

Team 기준으로 볼때 다대일의 반대인 일대다 관계가 되므로 `@OneToMany` 를 사용하여 Member 와의 관계 매핑을 한다.

**@OneToMany 주요 속성**

- `mappedBy` - 양방향 매핑 시 반대쪽 매핑 **필드의 이름**을 값으로 지정해야한다. 이 경우에는 반대쪽 매핑핑이 `Member.team` 이기 때문에 "team"을 값으로 넣었다.

