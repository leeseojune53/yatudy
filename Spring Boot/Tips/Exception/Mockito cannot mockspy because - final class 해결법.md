# Mockito cannot mock/spy because - final class ํด๊ฒฐ๋ฒ

### ๐ ๋ฌธ์ ์ํฉ

**Test ๊ตฌ๋ ์** `Mockito cannot mock/spy because - final class` ์์ธ๊ฐ ๋ฐ์ํ๋ค.

### ๐ดโโ ๏ธ ์์ธ

Mockito๊ฐ **final class๋ฅผ Mocking**ํ๋ ค๊ณ ํด์ ๋ฐ์

### โป ํด๊ฒฐ๋ฒ

build.gradle.kts์ `testImplementation("org.mockito:mockito-inline:2.13.0")` ์ถ๊ฐ

`mockito-inline`์ `mockito`์คํ์ ์ธ ๊ธฐ๋ฅ์ ๋ฏธ๋ฆฌ ์ฌ์ฉํ  ์ ์๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์๋๋ค.
