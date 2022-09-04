# JPA Repository Unable to locate Attribute 발생

### 🐛 문제상황

```java
public class WeekendMealApplyEntity extends BaseEntity {

  ...
  
    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "weekend_meal_id")
    private WeekendMealEntity weekendMeal;

    public UUID getWeekMealId() {
        return this.weekendMeal.getId();
    }
  
  ...

}

public abstract class BaseEntity {

    @Id
    @GeneratedValue(generator = "uuid2")
    @GenericGenerator(name = "uuid2", strategy = "uuid2")
    @Column(columnDefinition = "BINARY(16)")
    private UUID id;

    @Column(columnDefinition = "BINARY(16)")
    private UUID userId;

    @NotNull
    @CreatedDate
    private LocalDate date;
}


```



`lang.IllegalArgumentException: Unable to locate Attribute  with the the given name [weekendMealId] on this ManagedType [io.github.v1serviceapplication.global.entity.BaseEntity]`

위의 에러가 발생한다.

### 🏴‍☠️ 원인

일반적인 field가 존재하지 않아서 발생하는 문제가 아니라 `getWeekMealId` 라는 method가 있어서 JPA에서 query method를 만들 때 오류가 발생한 것이다.

### ♻️ 해결법

`getWeekMealId`의 이름을 `getMealId`와 같이 다른 이름으로 변경한다.