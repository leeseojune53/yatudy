# Nginx로 소켓 Reverse Proxy

### 🎊 시작하기 전에...

우선 [Proxy](https://github.com/leeseojune53/yatudy/blob/main/Server/Proxy%EB%9E%80.md)에 대한 기본적인 지식이 있어야 합니다.

### ♻️ 해결법

```
location / {
	...
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection "Upgrade";
	...
}
```

위의 proxy_set ~~를 추가해주면 socket 연결이 된다.

이렇게 해야하는 이유는 reverse proxy 해줄 때 요청을 가로채는데, 이 때 적절한 헤더를 포함해서 WAS에 요청을 보내야하므로 명시해준다.