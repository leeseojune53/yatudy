# ghcr(Github Container Registry)

### ๐ ์ ์

Github Container Registry(ghcr)๋ ๊นํ๋ธ์์ ์ ๊ณตํ๋ **์ปจํ์ด๋ ๋ ์ง์คํธ๋ฆฌ ์๋น์ค**์ด๋ค.

๋์ปคํ๋ธ์ ๊ฐ์ ๋ ์ง์คํธ๋ฆฌ ์๋น์ค์ด๋ค.

### 1๏ธโฃ ์ฒซ ๋ฒ์งธ๋ก

Github Personal Access Token์ ์์ฑํ๋ค.

Token์๋ repo, write:package, delete:package ์ ์ฒด ๊ถํ์ด ํ์ํ๋ค.

https://github.com/settings/tokens

### 2๏ธโฃ ๋ ๋ฒ์งธ๋ก

ํ์ผ์ ํด๋น ํ ํฐ์ ์ ์ฅํ๊ณ , 

`cat {FILE_NAME} | docker login ghcr.io -u leeseojune53 --password-stdin`

๋ฅผ ์ฌ์ฉํ๋ฉด `Login Succeeded`๊ฐ ์ถ๋ ฅ๋  ๊ฒ์ด๋ค.

> ๋ง์ฝ ์ ์ฅํ๊ธฐ ์ด๋ ค์ด ์ํฉ์ด๋ผ๋ฉด ์๋์ ๊ฐ์ด ์ฌ์ฉํด๋ ๋๋ค.

`docker login ghcrcr.io -u {GITHUB_USERNAME} --password {ACCESS_TOKEN}`

### 3๏ธโฃ ์ธ ๋ฒ์งธ๋ก

DockerHub๋ฅผ ์ฌ์ฉํ๋ฏ์ด docker push ๋ฐ pull๋ฐ์์ ์ฌ์ฉํ๋ฉด ๋๋ค.

pushํ ์ด๋ฏธ์ง๋ **๊นํ๋ธ ํ๋กํํ์ด์ง**์์ **packages**๋ฅผ ๋ณด๋ฉด ๋๋ค.
