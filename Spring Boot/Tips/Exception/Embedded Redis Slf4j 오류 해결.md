# Embedded Redis Slf4j ์ค๋ฅ ํด๊ฒฐ

### ๐ ๋ฌธ์  ์ํฉ

```LoggerFactory is not a Logback LoggerContext but Logback is on the classpath```

build.gradle ์ ```testImplementation 'it.ozimov:embedded-redis:0.7.3'```๋ฅผ ์ถ๊ฐํ๊ณ  ์คํํ๊ฒ๋๋ฉด ์์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค.

### ๐ดโโ ๏ธ ์์ธ

embedded-redis 0.7.3 ๋ฒ์ ์ ๋ฏ์ด๋ณด๋ฉด Slf4j๊ฐ ์ถ๊ฐ๋์ด์๋ค.

Spring Boot ๋ด์ค Slf4j๊ณผ ์ถฉ๋ํด์ ๋ฐ์ํ ๊ฒ์ด๋ค.

### โป ํด๊ฒฐ๋ฒ

embedded-redis๋ฅผ ๋ค์ด๊ทธ๋ ์ด๋ํ๋ ๊ฒ์ด๋ค.

```testImplementation 'it.ozimov:embedded-redis:0.7.2'``` ๋ก ๋ณ๊ฒฝํด์ฃผ๋ฉด Slf4j๊ฐ ์์ด์ง๋ฉด์ ํด๊ฒฐ๋๋ค.