# Log4j vs Slf4j

### ๐ ์์ํ๊ธฐ ์ ์..

ํ์๋ ์ด์ ์์ด **Slf4j**๋ฅผ ์ ์ฉํ์๋๋ฐ, Log4j์ ์ฐจ์ด์ ์ด ๊ถ๊ธํด ์ ๋ฆฌํ ๊ธ์ด๋ค.

### ๐ต ์ฐจ์ด์ 

Log4j๋ Logging ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ด๋ค.

Slf4j๋ ๋ค๋ฅธ Logging ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ **Wrappingํด๋์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ**์ด๋ค. ์ฆ, Slf4j๋ฅผ ์ฌ์ฉํ๋ฉด ๋ค๋ฅธ Logging ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ก **๋ฐ๊ฟ ๋ผ์ธ ์ ์๋ค๋ ๊ฒ**์ด๋ค.

๊ฐ๋จํ๊ฒ ๋งํ์๋ฉด Slf4j๋ **์ธํฐํ์ด์ค**์ด๋ค.

๊ฒฐ๋ก ์ Log4j๋ณด๋ค๋ Slf4j๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ด ์ข๋ค. ์๋ํ๋ฉด **ํ์ฅ์ฑ ์ธก๋ฉด**์์ Slf4j๊ฐ ์๋ฑํ๊ธฐ ๋๋ฌธ์ด๋ค.