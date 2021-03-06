# Spring Unit Test

### ๐ ์์ํ๊ธฐ์ ์..

์คํ๋ง์์ ์ ๋ ํ์คํธ๋ฅผ ์งํ ํ  ๋ ์๋ฐ์ mocking ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ธ mockito๋ฅผ ์์ฃผ ์ฌ์ฉํ๋ค. mockito๋ด์์๋ ์ฌ๋ฌ ๋ฐฉ๋ฒ์ด ์์ด ๊ทธ ์ค ์ต์ ์ ๋ฐฉ๋ฒ์ ๊ณ ๋ คํ๊ณ  ์์ฑํ ๊ธ์ด๋ค.

### 1๏ธโฃ Mocking ํ๋ ๋ฐฉ๋ฒ

Mockingํ๋ ๋ฐฉ๋ฒ์๋ ์ฌ๋ฌ ๋ฐฉ๋ฒ์ด ์กด์ฌํ๋ค.

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

์ ์ฝ๋๋ฅผ ๋ณด๋ฉด 1๋ฒ๊ณผ 2๋ฒ์ด ๋ณ ์ฐจ์ด ์์ด๋ณด์ธ๋ค. ํ์ง๋ง, Bean์ ์ฃผ์ํด์ผํ๋ค๋ฉด ์กฐ๊ธ ๋ฌ๋ผ์ง๋ค.

``` java
1.
~~~
UserService userService = new UserService(userRepository);
~~~

2.
@InjectMocks
UserService userService;
```

Bean์ ์ฃผ์ํ๋ ๋ถ๋ถ์ ๋ณด๋ 2๋ฒ ์ฝ๋๊ฐ ๋์ฑ ๊น๋ํด๋ณด์ธ๋ค.

**2๋ฒ ์ฝ๋๋ฅผ ๊ถ์ฅํ๋ค.**

#### โ 1๋ฒ์์๋ @InjectMocks๋ฅผ ์ฌ์ฉํ๋ฉด์๋ผ๋?

InjectMocks๋ JUnit Extension์ ๊ตฌํํ MockitoExtension์ ์ฌ์ฉํด์ผ ์ ์์ ์ผ๋ก ์๋ํ๋ฏ๋ก, 1๋ฒ ์ฝ๋์์๋ ์๋ํ์ง ์๋๋ค.

**MockitoExtension์ beforeEach, AfterEach ๋ฑ์ด ๊ตฌํ**๋์ด์๊ณ , Test ์คํ ์ ๋ถ๊ฐ์ ์ผ๋ก ์๋ํ๋ ๊ฒ์ด๋ค.

### 2๏ธโฃ ๊ฐ ๋น๊ตํ๋ ๋ฐฉ๋ฒ

Mocking ํ๋ ๋ฐฉ๋ฒ์๋ ์ฌ๋ฌ ๋ฐฉ๋ฒ์ด ์๋ฏ์ด ๊ฐ์ ๋น๊ตํ๋ ๋ฐฉ๋ฒ์๋ ์ฌ๋ฌ ๋ฐฉ๋ฒ์ด ์๋ค.

```java
1.
assertEquals(A, B)

2.
assertThat(A).isEqualTo(B)
```

1๋ฒ ๋ฐฉ๋ฒ์์ A๊ฐ ์ค์  ๊ฐ์ธ์ง, B๊ฐ ์ค์  ๊ฐ์ธ์ง ํท๊ฐ๋ฆด ์ ์๋ค. ํ์ง๋ง 2๋ฒ ๋ฐฉ๋ฒ์์๋ .isEqualTo๋ก ๊ตฌ๋ถ์ ํด์ ๋ ๊ฐ์ด ํท๊ฐ๋ฆฌ์ง ์๋๋ค.

**๋ฐ๋ผ์ 2๋ฒ ์ฝ๋๋ฅผ ๊ถ์ฅํ๋ค.**

### 3๏ธโฃ Stubing ํ๋ ๋ฐฉ๋ฒ

BDD์์๋ given, when, then ์ธ ๋จ๊ณ๋ก ๋๋ ์ ํ์คํธ๋ฅผ ์งํํ๋ค.

- given : ํ์คํธ์ ํ์ํ ๊ฐ์ ์ค์ ํ๋ค.
- when : ํ์คํธ์ ํ์ํ ์กฐ๊ฑด.
- then : ํ์คํธ์ ๊ฒฐ๊ณผ๋ฅผ ๊ฒ์ฆํ๋ค.

์ฌ๊ธฐ์ Mockito๋ฅผ ์ฌ์ฉํ๋ฉด given, ๊ฐ์ ์ค์ ํ๋ ๋ถ๋ถ์์ when์ ์ฌ์ฉํ๊ฒ๋๋ค. ๋ฐ๋ผ์ BDD๋ฅผ ํ๋ค๋ฉด Mockito๋ณด๋ค๋ BDDMockito๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ ๊ถ์ฅํ๋ค. ์๋๋ ๋ ๋ฐฉ๋ฒ์ ์ฐจ์ด์ ์ ๋ณด์ฌ์ฃผ๋ ์ฝ๋์ด๋ค.

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

1๋ฒ ์ฝ๋๋ฅผ ๋ณด๋ฉด given์์ญ์ when method๊ฐ ๋ค์ด๊ฐ์์ด์ ์ ์ง ๋ถํธํ๊ณ  ํท๊ฐ๋ฆฐ๋ค. ๊ทธ์๋นํด 2๋ฒ ์ฝ๋๋ฅผ ๋ณด๋ฉด given์์ญ์ ์๋ง๋ given method๊ฐ ์์ด์ ์ง๊ด์ ์ด๋ค.

**๋ฐ๋ผ์ 2๋ฒ ์ฝ๋๋ฅผ ๊ถ์ฅํ๋ค.**

2๋ฒ ์ฝ๋๋ฅผ ์ฌ์ฉํ๋ ค๋ฉด `BDDMockito`๋ฅผ import ํด์ค์ผํ๋ค.