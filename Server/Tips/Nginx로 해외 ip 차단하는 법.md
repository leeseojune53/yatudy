# Nginx로 해외 ip 차단하는 법

### 🎊 시작하기 전에..

해외 ip를 차단하려면 필요한 module가 있다. `--with-http_geoip_module`가 있어야 하는데 `nginx -V`를 사용해 확인한 후 아래를 보면 좋을 것 같다.

### 1️⃣ 가장 먼저

ubuntu 기준으로 `/etc/nginx` 내부에 있는 `nginx.conf` 파일의 http안에 값을 넣어준다.

```
geoip_country /usr/share/GeoIP/GeoIP.dat;
map $geoip_country_code $allowed_country {
	default no;
	KR yes;
}
```

$geoip_country_code가 **KR이면** $allowed_country에 **yes**의 값이 들어가고, **이외에는 no**의 값이 들어간다.

### 2️⃣ 두 번째로

사용할 server에 if문으로 `$allowed_country`를 검증한다.

```
server {
	...
	
	if ($allowed_country = no) {
		return 412;
	}
	
	...
}
```

위와 같이 하면 된다.

### ⚠ 주의점

if($\~\~)를 하게되면 에러가 발생하는데, 문법상 if ($\~\~)로 해야하기 때문이다.



만약, `nginx: [emerg] unknown directive "geoip_country"`가 뜬다면 [링크](https://stackoverflow.com/questions/36554405/how-to-enable-dynamic-module-with-an-existing-nginx-installation)를 참고하면된다. 간단하게 동적 모듈이 없어서 발생하는 문제이다.
