# 상속에서 @Builder 사용하기

### 🐛 문제 상황

```error: builder() in 자식객체 cannot hide builder() in 부모객체```

라는 에러가 나오게 된다. 간단하게 직역하면 "자식객체의 builder()는 부모객체의 builder()에 숨을 수 없다."라는 말인데 이것을 해결하려면 **상속**에 대한 기본적인 개념이 있어야한다.



### 🏴‍☠️ 원인

Builder 어노테이션을 사용하면 컴파일 시 해당 클래스에 Builder 클래스와 메소드가 생기게 된다. 

자식 객체에서 동일하게 Builder 어노테이션을 사용하게 되면  builder() 메소드가 **중복**되게 되면서 error가 발생하게 된다.



### ♻ 해결법

자식객체 또는 부모객체에서 @Builder어노테이션의 property중 하나인 **builderMethodName**를 설정해주면 된다. 물론 사용할 때는 builderMethodName에서 지정해준 method 명을 사용해서 호출해야 한다.



### 😎 예제

```java
@Getter
@Builder
@AllArgsConstructor
@JsonInclude(JsonInclude.Include.NON_NULL)
public class FeedResponse {

    private Long feedId;
    private String title;
    private String description;
    private Integer price;
    private List<String> tags;
    private String photo;
    private LocalDateTime lastModifyDate;
    private boolean like;
    private Integer count;

    public void setLike(boolean value) {
        this.like = value;
    }

}
```

```java
@Getter
@JsonInclude(JsonInclude.Include.NON_NULL)
public class WriteFeedResponse extends FeedResponse {

    private Integer headCount;
    private LocalDate date;
    private boolean isUsedItem;

    @Builder(builderMethodName = "writeFeedResponseBuilder")
    public WriteFeedResponse(Long feedId, String title, String description,
			Integer price, List<String> tags, String photo,
			LocalDateTime lastModifyDate, boolean like, Integer count,
			Integer headCount, LocalDate date, boolean isUsedItem) {
    	super(feedId, title, description, price,
				tags, photo, lastModifyDate, like, count);
    	this.headCount = headCount;
    	this.date = date;
    	this.isUsedItem = isUsedItem;
	}

    public void setGroupFeed(Integer headCount, LocalDate date) {
        this.headCount = headCount;
        this.date = date;
    }

    public void setLike(boolean value) {
        super.setLike(value);
    }

}

```

아래는 실제 메소드 호출 시 사용법이다.

```java
WriteFeedResponse feedResponse = 
    WriteFeedResponse.writeFeedResponseBuilder()
    .build();
    
```