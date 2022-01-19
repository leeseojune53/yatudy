# 요청을 보냈을 때 Filter에서 발생한 Exception 때문에 CORS가 뜰 때 해결법

### 🐛 문제 상황

```Access to XMLHttpRequest at 'https://server.music-ward.com/users/me' from origin 'http://localhost:3000/' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.```

Exception이 발생하지 않으면 정상적으로 작동하지만 Exception이 **발생**하면 CORS 오류가 발생한다..

### 🏴‍☠️ 원인

Response에 CORS 세팅이 들어가기 전에 Exception이 발생해서 Response에 **CORS관련 헤더**가 들어가지 않음.

### ♻ 해결법

Exception이 발생하는 Filter 앞에 Filter를 만들어 준다.

Request -> **Exception Filter** -> Dispatcher Servlet 일 때

Request -> Cors Filter -> Exception Filter -> Dispatcher Servlet로 설계해준다.

### 😎 예제

CorsFilter

```java
HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse res = (HttpServletResponse) response;
		res.setHeader("Access-Control-Allow-Origin", "http://localhost:3000");
		res.setHeader("Access-Control-Allow-Origin", "https://~~~");
		res.setHeader("Access-Control-Allow-Credentials", "true");
		res.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE, HEAD");
		res.setHeader("Access-Control-Max-Age", "3600");
		res.setHeader("Access-Control-Allow-Headers", "Content-Type, Accept, X-Requested-With, Authorization");
		chain.doFilter(req, res);
```

Filter 내부에 위의 코드를 사용해준다.

Filter은 ```addFilterBefore```를 활용해서 Exception이 발생하는 Filter앞에 배치해준다.