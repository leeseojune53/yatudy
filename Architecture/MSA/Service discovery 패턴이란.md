# Service discovery ํจํด์ด๋?

### ๐ ์ ์

ํด๋ผ์ด์ธํธ๊ฐ ์๋น์ค๋ฅผ ํธ์ถํ  ๋ ์๋น์ค์ ์์น๋ฅผ ์์๋ผ ์ ์๋ ๊ธฐ๋ฅ์ ๋งํ๋ค.

### โ Service Registery

๊ฐ ์๋น์ค์ ์ ๋ณด(IP, ํฌํธ๋ฒํธ)๋ฅผ ๊ฐ์ง๊ณ ์๋ Registery์ด๋ค.

Spring Cloud์ Eureka๋ Consul์ ๊ฐ์ ์๋น์ค์ด๋ค.

### 1๏ธโฃ Client side discovery

<img src="https://t1.daumcdn.net/cfile/tistory/99E912455AD610DE09" alt="img" style="zoom:50%;" />

ํด๋ผ์ด์ธํธ๊ฐ Service registery๋ฅผ ํธ์ถํด์ ์๋น์ค์ ์์น๋ฅผ ํ์ธํ๊ณ  ํธ์ถํ๋ ๊ฒ.

### 2๏ธโฃ Server side discovery

<img src="https://t1.daumcdn.net/cfile/tistory/99AF813E5AD610DE03" alt="img" style="zoom:50%;" />

์๋น์ค ์์ ๋ก๋๋ฐธ๋ฐ์๋ฅผ ๋ฐฐ์นํจ์ผ๋ก์จ ํด๋ผ์ด์ธํธ๊ฐ ์๋น์ค๋ฅผ ํธ์ถํ๋ฉด ๋ก๋๋ฐธ๋ฐ์๊ฐ Service registery๋ฅผ ํธ์ถํ๊ณ , ํด๋น ์ ๋ณด๋ก ๋ผ์ฐํํ๋ ๋ฐฉ์์ด๋ค.



## Reference

[MSA์์ Service discovery ํจํด](https://bcho.tistory.com/1252)