# Spring Boot Exception 효율적인 처리방식

[홍정현](https://github.com/jhhong0509)감사합니다!

### 🎊 시작하기 전에..

일반적으로 Spring Boot에서 Exception을 처리하면, `@ControllerAdvice`를 이용해서 Exception을 Handling하는데, Exception을 throw할 때 `throw new ~~Exception()`로 throw한다.

위 상황으로 사용하고 있다고 가정하고 글을 읽어주기 바란다.

### ❓ 효율적으로 바꿔야하는 이유

Exception을 새로 생성할 때 trace가 생성되며 성능에 영향을 미칩니다. catch하지 못한 Exception이라면 trace가 필요하겠지만, Client Exception을 발생시킬 때는 의도하고 발생시킨 것이므로, trace가 예외를 만들 때마다 필요하지 않습니다.

### ♻ 효율적인 방법

Exception을 static variable로 만들어 Exception을 재사용 하는 방법입니다.

```java
public class CustomException extends ServerException {
    
    public static final EXCEPTION = 
        new CustomException();
    
    private CustomException() {
        super(ErrorCode.CUSTOM_EXCEPTION);
    }
    
}
```

위와같이 class를 만들고, 사용하는 곳에서는 기존에 throw new가 아닌 throw를 합니다.

```java

public class UserService {
    
    public void setInformation(String name) {
        ~~~
        throw new CustomException(); // 기존 방법
        throw CustomException.EXCEPTION; // 보다 효율적인 방법
    }
    
}


```

