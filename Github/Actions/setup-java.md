# Setup-java

#### ๐ ์ ์

Github Actions workflow์์ **์๋ฐ๋ฅผ ์ธํ**ํ๋ Github Action

### ์ฌ์ฉ๋ฒ

```yaml
steps:
- uses: actions/setup-java@v2
  with:
    distribution: '๊ตฌํ์ฒด ์ข๋ฅ'
    java-version: '๋ฒ์ '
```

๊ตฌํ์ฒด ์ข๋ฅ๋ `temurin`, `zulu`, `adopt`, `adopt-openj9`, `liberica`, `microsoft`๊ฐ ์๋ค.

๋ฒ์ ์ major๋ง ๋ณด๋ฉด `8`, `11`, `16`, `17`์ด ์๋ค.