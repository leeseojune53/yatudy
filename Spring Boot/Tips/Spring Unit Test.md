# Spring Unit Test

### 🎊 시작하기전에..

스프링에서 유닛 테스트를 진행 할 때 자바의 mocking 라이브러리인 mockito를 자주 사용한다. mockito내에서도 여러 방법이 있어 그 중 최선의 방법을 고려하고 작성한 글이다.

### 1️⃣ Mocking 하는 방법

Mocking하는 방법에는 여러 방법이 존재한다.

``` java
1.
import static org.mockito.Mockito.mock;

class UserServiceTest {
  UserRepository userRepository = mock(UserRepository.class);
}

2.
import org.mockito.Mock;

@ExtendWith(MockitoExtension.class)
class UserServiceTest {
  @Mock
  UserRepository userRepository
}
```

위 코드를 보면 1번과 2번이 별 차이 없어보인다. 하지만, Bean을 주입해야한다면 조금 달라진다.

``` java
1.
~~~
UserService userService = new UserService(userRepository);
~~~

2.
@InjectMocks
UserService userService;
```

Bean을 주입하는 부분을 보니 2번 코드가 더욱 깔끔해보인다.

**2번 코드를 권장한다.**

#### ❓ 1번에서도 @InjectMocks를 사용하면안돼나?

InjectMocks는 JUnit Extension을 구현한 MockitoExtension을 사용해야 정상적으로 작동하므로, 1번 코드에서는 작동하지 않는다.

**MockitoExtension은 beforeEach, AfterEach 등이 구현**되어있고, Test 실행 시 부가적으로 작동하는 것이다.

### 2️⃣ 값 비교하는 방법

Mocking 하는 방법에도 여러 방법이 있듯이 값을 비교하는 방법에도 여러 방법이 있다.

```java
1.
assertEquals(A, B)

2.
assertThat(A).isEqualTo(B)
```

1번 방법에서 A가 실제 값인지, B가 실제 값인지 헷갈릴 수 있다. 하지만 2번 방법에서는 .isEqualTo로 구분을 해서 두 값이 헷갈리지 않는다.

**따라서 2번 코드를 권장한다.**

### 3️⃣ Stubing 하는 방법

BDD에서는 given, when, then 세 단계로 나눠서 테스트를 진행한다.

- given : 테스트에 필요한 값을 설정한다.
- when : 테스트에 필요한 조건.
- then : 테스트의 결과를 검증한다.

여기서 Mockito를 사용하면 given, 값을 설정하는 부분에서 when을 사용하게된다. 따라서 BDD를 한다면 Mockito보다는 BDDMockito를 사용하는 것을 권장한다. 아래는 두 방법의 차이점을 보여주는 코드이다.

```java
1.
@Test
void test() {
  //given
  User user = new User();
  when(userRepository.queryUser())
    .thenReturn(user);
  
  //when
  userService.execute();
  
  //then
  assertThat()~~
  
}
```

```java
2.
@Test
void test() {
  //given
  User user = new User();
  given(userRepository.queryUser()).willReturn(user);
  
  //when
  userService.execute();
  
  //then
  assertThat()~~
  
}
```

1번 코드를 보면 given영역에 when method가 들어가있어서 왠지 불편하고 헷갈린다. 그에비해 2번 코드를 보면 given영역에 알맞는 given method가 있어서 직관적이다.

**따라서 2번 코드를 권장한다.**

2번 코드를 사용하려면 `BDDMockito`를 import 해줘야한다.