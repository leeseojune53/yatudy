# Nginx๋ก ์์ผ Reverse Proxy

### ๐ ์์ํ๊ธฐ ์ ์...

์ฐ์  [Proxy](https://github.com/leeseojune53/yatudy/blob/main/Server/Proxy%EB%9E%80.md)์ ๋ํ ๊ธฐ๋ณธ์ ์ธ ์ง์์ด ์์ด์ผ ํฉ๋๋ค.

### โป๏ธ ํด๊ฒฐ๋ฒ

```
location / {
	...
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection "Upgrade";
	...
}
```

์์ proxy_set ~~๋ฅผ ์ถ๊ฐํด์ฃผ๋ฉด socket ์ฐ๊ฒฐ์ด ๋๋ค.

์ด๋ ๊ฒ ํด์ผํ๋ ์ด์ ๋ reverse proxy ํด์ค ๋ ์์ฒญ์ ๊ฐ๋ก์ฑ๋๋ฐ, ์ด ๋ ์ ์ ํ ํค๋๋ฅผ ํฌํจํด์ WAS์ ์์ฒญ์ ๋ณด๋ด์ผํ๋ฏ๋ก ๋ช์ํด์ค๋ค.