# DI(Dependency Injection)๋?

### ๐ ์ ์

์์กด์ฑ ์ฃผ์์ด๋ผ๊ณ  ๋งํ๋ฉฐ, ์ถ์ํ๋ฅผ ํด์น์ง ์๊ณ  **์์กด์ฑ**์ **์ธ์๋ก ๋๊ฒจ์ฃผ๋ ๊ฒ**์ ๋งํ๋ค.

### ๐ ์์

Spring Boot๋ฅผ ์์๋ก ๋ค์๋ฉด, Service ๊ตฌํ์ฒด์์๋ Repository์ ๊ฐ์ด **์์กด์ฑ์ ๊ฐ๋๋ค**. ํ์ง๋ง ํด๋น Service๋ฅผ ์ถ์ํ ํ interface๋ Repository๊ฐ ์ธ์๋ก ๋์ด๊ฐ๋ **์ ๋ณด๋ฅผ ์ ํ ๋ด๊ณ ์์ง ์๋ค**.

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

์ ์ฝ๋๋ ์์ ๋ง์ ๋ฐ๋ผ์ UserServiceImpl์ ์ถ์ํ ๊ฐ์ฒด์ธ UserService์๋ UserRepository์ ์ ๋ณด๋ฅผ ์ ํ ๋ด๊ณ  ์์ง์์ ๊ฒ์ ๋ณผ ์ ์๋ค.

### โ  ์ฃผ์์ 

์ฌ๊ธฐ์ ์ฐฉ๊ฐํ๋ ๋ถ๋ถ์ ๋ด๊ฐ ์์์ ์ค๋ชํ ๊ฒ์ ๋ณด๊ณ  DI๋ฅผ ๊ตฌํํ๋ ค๋ฉด interface๋ฅผ ๊ผญ ๊ฐ์ ธ์ผ ํ๋ค๊ณ  ์๊ฐํ๋ ๊ฒ์ด๋ค.

์ฌ๊ธฐ์ ํผ๋ํ  ์ ์๋ ๊ฐ๋์ด DIP(Dependency Injection Principle)์ธ๋ฐ DIP์๋ ๋ช๋ฐฑํ๊ฒ ๋ค๋ฅธ ๊ฒ์ด๋ค. ์์ธํ ๋ด์ฉ์ [DI vs IoC vs DIP](https://github.com/leeseojune53/yatudy/blob/main/OOP/DI%20vs%20IoC%20vs%20DIP.md)๋ฅผ ์ฐธ๊ณ ํด์ฃผ์ธ์!