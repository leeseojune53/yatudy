# DI(Dependency Injection)란?

### 📌 정의

의존성 주입이라고 말하며, 추상화를 해치지 않고 **의존성**을 **인수로 넘겨주는 것**을 말한다.

### 😎 예시

Spring Boot를 예시로 들자면, Service 구현체에서는 Repository와 같이 **의존성을 갖는다**. 하지만 해당 Service를 추상화 한 interface는 Repository가 인수로 넘어가는 **정보를 전혀 담고있지 않다**.

```java
interface UserService {
    void removeUser(Long userId);
}
```

```java
@Service
@RequiredArgsConstructor
class UserServiceImpl implements UserService {
    private final UserRepository userRepository;
}
```

위 코드는 위의 말에 따라서 UserServiceImpl의 추상화 객체인 UserService에는 UserRepository의 정보를 전혀 담고 있지않은 것을 볼 수 있다.

### ⚠ 주의점

여기서 착각하는 부분은 내가 위에서 설명한 것을 보고 DI를 구현하려면 interface를 꼭 가져야 한다고 생각하는 것이다.

여기서 혼동할 수 있는 개념이 DIP(Dependency Injection Principle)인데 DIP와는 명백하게 다른 것이다. 자세한 내용은 [DI vs IoC vs DIP](https://github.com/leeseojune53/yatudy/blob/main/OOP/DI%20vs%20IoC%20vs%20DIP.md)를 참고해주세요!