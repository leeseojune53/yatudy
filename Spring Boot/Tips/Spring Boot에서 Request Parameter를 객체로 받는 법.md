# Spring Boot에서 Request Parameter를 객체로 받는 법.

``` java
@Getter
@Setter
public class QueryRequest {
    
    private userName;
    private userEmail;
    
}
```

```java
@RestController
@RequiredConstructor
public class QueryController {
    
    private final UserService;
    
    @GetMapping("/")
    public QueryResponse QueryTest(QueryRequest query) {
        return userService.UseQueryMethod(query);
    }
    
}
```

주의 사항으로는 QueryRequest에 존재하는 필드의 명과 QueryString으로 오는 필드의 명이 동일해야 한다는 점이다. 따라서 `@JsonAlias("user_name")`과 같이 사용하는 경우도 존재한다.