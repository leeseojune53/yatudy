# Entity에서 식별관계 매핑하기

### 🎊 시작하기 전에..

**식별관계**와 **비식별관계**에 대한 이해도가 있으면 더 좋다.

### ♻ 매핑하는 법

기존처럼 `@OneToOne` 또는 `@OneToMany`를 사용하되 조금 더 처리를 해줘야한다.

일단 관계 매핑을 하는 어노테이션(`@OneToOne`, `@OneToMany`)에 `@MapsId`를 붙여준다. 그리고 **PK로 사용할 값**을 따로 **명시**해준다.

말로만으로는 이해가 어려우니 아래 코드를 참고하길 바란다.

```java
class Score {
    @Id
    private Long userId; 				//User의 PK
    
    @MapsId
    @OneToOne
    @JoinColumn(name = "user_id")
    private User user;				연관관계 매핑
}


@Entity
class User {
    @Id	@GeneratedValue
    private Long id;
}
```

