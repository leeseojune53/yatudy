# Spring Boot Admin ๊ตฌ์ถํ๊ธฐ

### ๐ ์์ํ๊ธฐ์ ์...

Spring Boot Admin์ ๋ํด์ ๋๋ต์ ์ผ๋ก ํ์ํ๋๊ฒ ์ข์ ๊ฒ ๊ฐ๋ค.

๋, Spring Boot Admin์ ๊ตฌ์ถํ๋ ค๋ฉด ํ๋ ์ด์์ Client๊ฐ ์กด์ฌํด์ผํ๋ค.

**ํ์ฌ Kotlin์์๋ ์๋ํ์ง ์๋๊ฒ์ผ๋ก ๋ณด์ธ๋ค.**

### 1๏ธโฃ ๊ฐ์ฅ ๋จผ์ 

Spring Boot Admin ํ๋ก์ ํธ๋ฅผ ๋ง๋ค์ด์ฃผ๊ณ , ๋ฐ์ ์์กด์ฑ์ ์ถ๊ฐํด์ค๋ค.

`implementation 'de.codecentric:spring-boot-admin-server:${version}'`

๊ทธ๋ฆฌ๊ณ , Client ํ๋ก์ ํธ์ ์๋์ ์์กด์ฑ์ ์ถ๊ฐํด์ค๋ค.

`implementation 'de.codecentric:spring-boot-admin-client:${version}'`

### 2๏ธโฃ ๋ ๋ฒ์งธ๋ก

Spring Boot Admin์ yml์ ์๋์ ๊ฐ์ด ์ค์ ํ๋ค.

```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

Spring Boot Client๋ ์๋์๊ฐ์ด ์ค์ ํ๋ค.

```yaml
spring:
  boot:
    admin:
      client:
        url: http://localhost:8000

management:
  endpoints:
    web:
      exposure:
        include: "*"
server:
  port: 18080
```



### โ ๋์ผ๋ก

์์ ์ฝ๋๋ ์ ๋ง Spring Boot Admin์ ๋ง๋ณด๋ ์ฝ๋์ด๋ค. ์ค์ ๋ก๋ Spring Security๋ฅผ ์ฌ์ฉํด์ ์ ๋ณด ์ ๊ทผ์ ์ ํํด์ผํ๋ค.