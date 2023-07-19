# 마지막 수정한 사람 저장하기(LastModifiedBy)

### 🎊 시작하기 전에...

JPA와 Spring Security를 사용해서 마지막으로 Entity를 수정한 사람이 누구인지 기록하는 `LastModifiedBy`에 관한 글이다.

또, AccountEntity로 기록하였지만, Id값만 기록한다면 AuditorAware의 Generic을 수정하여 사용하면 된다.

### 설정하는 방법

```java

@EnableJpaAuditing(auditorAwareRef = "auditorAware") //이 어노테이션을 SpringApplication에 선언하였으면 해당 부분을 수정하면 된다.
public class AuditorAwareImpl implements AuditorAware<AccountEntity> {
    @Override
    public Optional<AccountEntity> getCurrentAuditor() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        if (authentication == null || !authentication.isAuthenticated()) {
            return Optional.empty();
        }
        return Optional.of(AccountEntity.builder().id(authentication.getName()).build());
    }
}
```



### 사용하는 방법

Entity class의 상단에 EntityListeners에 AuditingEntityListener를 설정해준다.

정보를 변경한 사용자를 저장하는 컬럼에 `@LastModifiedBy`를 선언한다.

```java
...
@EntityListeners(AuditingEntityListener.class)
class SeoJuneEntity {
  
  ...
  @LastModifiedBy
	@JoinColumn(name = "updated_by")
	@ManyToOne(fetch = LAZY)
	private AccountEntity updatedBy;
}


```





