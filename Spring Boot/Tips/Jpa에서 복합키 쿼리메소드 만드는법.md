# Jpa에서 복합키 쿼리메소드 만드는법

### 🎊 시작하기 전에...

쿼리 메소드는 `findBy~` 와 같이 직접 쿼리를 짜는 것이 아닌, 메소드 명으로 쿼리를 만드는 것이다.

**Data Jpa**에서 제공하는 기능이다.

이 글에서 설명하는 내용은 아래 엔티티를 기반으로 설명한다. 또, 쿼리메소드는 UserRepository 내부에 있는 것으로 생각한다.

```java
@Entity
class Country {
  ...
  @Id
  private int id;
  ...
}

@Embeddable
class UserId implements Serializable {
  
  private int userId;
  
  @ManyToOne(fetch = FetchType.LAZY, optional = false)
  @JoinColumn(name = "counter_id")
  private Country country;
  
}

@Entity
class User {
  
  @EmbeddedId
  private UserId userId;
  
  ...
  
}
```

### 1️⃣ 복합키 내부 일반 컬럼

일반 컬럼을 findBy로 조회하는 방법은 `findByUserIdUserId`이다.

천천히 풀어보자면, findByUserId는 UserId라는 복합키를 이용해서 유저를 조회한다는 뜻인데, 여기서 UserId 내부 **필드**인 userId의 값으로 조회한다는 것을 UserIdUserId(복합키 필드명 + 복합키 내부 필드명)으로 만든 것이다.

### 2️⃣ 복합키 내부 FK

위 엔티티의 복합키에 Country와 관계가 맺어져있는데 일반 컬럼을 조회하듯이 하면 `findByUserIdCountry`로 할 수 있다. 하지만 이렇게 한다면 파라미터로 `Country`를 받게된다. 따라서 Country엔티티의 PK로 조회하고싶다면 `findByUserIdCountryId`로 하면 된다. 이 메소드 명을 풀면 (복합키 필드명 + 복합키 내부 필드명 + 복합키 내부 필드(Country)의 필드)로 된다.