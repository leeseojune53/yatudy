# JUnit5에서 테스트 순서 정하기

https://koko8829.tistory.com/2003

### 🎊 시작하기 전에..

**JUnit4에서는 다른 방법**을 사용해야 한다.

### ♻ 순서 정하는 법

```java
@TestMethodOrder(OrderAnnotation.class)
public class JUnitTest {
    
    @Test
    @Order(1)
    void TEST_1() {
        
    }
    
    @Test
    @Order(2)
    void TEST_2() {
        
    }
    
}
```

