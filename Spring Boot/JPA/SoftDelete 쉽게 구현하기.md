# SoftDelete 쉽게 구현하기

### 🎊 시작하기 전에...

JPA(Hibernate)를 사용해서 SoftDelete를 쉽게 기록할 수 있는 `SQLDelete`, `Where`에 관한 글이다.

### 설정하는 방법

```java

@SQLDelete(sql = "UPDATE feed SET deleted_at = NOW() WHERE feed_id = ?")
@Where(clause = "deleted_at IS NULL")
@Entity
public class FeedEntity {
    
    private String feedId;

    private UserEntity user;

    private String content;

    private LocalDateTime deletedAt;

}
```

만약 SoftDelete를 해야하지만, Application 상에서 불러올 일이 있다면 `@Where`를 제거하고, 사용하면 된다.
