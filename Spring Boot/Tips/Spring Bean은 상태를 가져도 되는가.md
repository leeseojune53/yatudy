# Spring Bean은 상태를 가져도 되는가?

### 🎊 시작하기 전에..

EntryDSM에서 Entry 6.0을 작업하면서, 중간에 생겼던 오류 중 pdf 미리보기에서 다른 사용자의 사진이 보여지는 현상이 있었는데 QA를 진행하면서 원인을 찾았다.

해당 코드를 짧게 보자면

```java
@Component
class PdfConverter {
    
    private final Map<String, Object> values = HashMap<>();
    
    ...
}
```

와 같은 구조였는데 **Bean은 상태를 가지면 안된다**. 위의 코드에서는 Bean이 **싱글톤**이므로 요청이 들어와도 **이전의 인스턴스를 그대로 사용**하는데 이 상황에서 **values까지 그대로 있었으므로 덮어쓰기 되지 않는 부분은 이전 사람의 값이 남아있는 것**이었다. 따라서 Spring Bean은 **State less**해야한다는 것을 깨닫고 이 글을 작성하게되었다.

### 📌 Spring Bean이란?

Spring Bean이란 Spring IoC Container(Application Context)가 관리하는 자바 객체를 Spring Bean이라 합니다.

### 😵 Spring Bean이 상태를 가지면 안되는 이유

위에서 서술했던 예시처럼 생각치 못한 크리티컬한 오류가 발생할 수 있다, 또한 상태를 가지게 된다면 Thread-Safe하지 않는다.

### ❓ 상태가 뭔데?

값이 변할 수 있는 **인스턴스 멤버 변수**가 존재하는 것.