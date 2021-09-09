# Spring Boot에서 UUID 사용하기

### 🐛 문제 상황

엔티티를 찾을 때 UUID로 조회를 할 때 UUID로 조회가 되지않는다.

### 🏴‍☠️ 원인

UUID의 길이는 16바이트의 길이를 요구한다.

MySQL에서 저장할 때 남는 길이는 **RPAD 처리**하고 저장한다.

### ♻ 해결법

엔티티에 @Column 어노테이션을 활용해서 ```columnDefinition = "BINARY(16)"``` 를 기입해주면 남는 칸이 없으므로 잘 검색이 된다.

### 😎 예제

```java
@Entity
class User {
    
    @Id
    @GeneratedValue(generator = "uuid2")
    @GenericGenerator(name = "uuid2", strategy = "uuid2")
    private UUID id;
    
}
```

를 아래와 같은 코드로 변경하면 해결된다.

```java
@Entity
class User {
    
    @Id
    @GeneratedValue(generator = "uuid2")
    @GenericGenerator(name = "uuid2", strategy = "uuid2")
    @Column(columnDefinition = "BINARY(16)")
    private UUID id;
    
}
```



