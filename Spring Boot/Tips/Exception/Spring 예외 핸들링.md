# Spring 예외 핸들링

`ResponseEntityExceptionHandler`를 핸들러에서 상속받으면, `ResponseEntityExceptionHandler`이 기본적으로 Spring MVC의 예외들을 핸들링 해준다.

핸들링 해주는 예외는 아래와 같다.

```
HttpRequestMethodNotSupportedException
HttpMediaTypeNotSupportedException
HttpMediaTypeNotAcceptableException
MissingPathVariableException
MissingServletRequestParameterException
ServletRequestBindingException
ConversionNotSupportedException
TypeMismatchException
HttpMessageNotReadableException
HttpMessageNotWritableException
MethodArgumentNotValidException
MissingServletRequestPartException
BindException
NoHandlerFoundException
AsyncRequestTimeoutException
```

### 😎 예시

```java
@ControllerAdvice
public class RestExceptionHandler extends ResponseEntityExceptionHandler {
  ...
}
```

위 와 같이 사용하면 Spring MVC의 예외들은 ResponseEntityExceptionHandler에서 처리한다. 만약 변경하고 싶으면 Override하면 된다.

`@ControllerAdvice`를 사용한 이유는 상속받은 class에서 ResponseEntity로 반환하므로 굳이 `ResponseBody`가 필요없기 때문이다.