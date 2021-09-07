# @Query에서 FK로 객체를 찾는 법



```java
class User {
    @Id
    private String username;
    
    @ManyToOne
    private School school;
}
```

```java
class School {
    @Id
    private String schoolName;
    
    @OneToMany
    private Set<User> users;
}
```





```java
@Query(select u from User u where user.school = :school)
List<User> findBySchool(@Param('schoolName')String schoolName);
```

위 Repository interface는 얼핏 보면 그럴싸 해보이지만 실제로 구동을 시켜보면 500에러와 함께 application이 소리를 지른다. 그 이유는 school은 객체로 맵핑이 되어있기 때문인데 실제 작동을 시키려면

```java
@Query(select u from User u where user.school.schoolName = :school)
List<User> findBySchool(@Param('schoolName')String schoolName);
```

위와 같이 school객체(모델)의 안에 있는 schoolName으로 맵핑을 해주어야 schoolName으로 FK객체의 값으로 찾을 수 있다.



## 도움 주신 분

[박상우](https://github.com/parksangwoo1617)