# Request Dto에서 @Builder 사용 시 이상한 점

### 🐛 문제 상황

Request Dto 객체에서 **Field가 하나일 때** `@NoargsConstructor`를 사용하지 않고 `@Builder`와 `@Getter`만 사용하면 

```
Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot construct instance of `io.github.tn1.server.dto.chat.request.JoinRequest` (although at least one Creator exists): cannot deserialize from Object value (no delegate- or property-based Creator); nested exception is com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot construct instance of `io.github.tn1.server.dto.chat.request.JoinRequest` (although at least one Creator exists): cannot deserialize from Object value (no delegate- or property-based Creator)
 at [Source: (PushbackInputStream); line: 2, column: 5]]
```

위와같은 예외가 발생한다.



### ❓ 이상한 점

**Field가 두 개 이상**일때는 이러한 예외가 발생하지 않는다.



위와 같은 문제의 원인을 찾아보려 했으나 실패했습니다. 😭😭 혹시 원인을 알게되시는 분 있으시면 PR 날려주세요!