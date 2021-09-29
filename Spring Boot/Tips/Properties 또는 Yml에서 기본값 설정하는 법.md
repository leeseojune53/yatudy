# Properties 또는 Yml에서 기본값 설정하는 법

### ❓ 필요한 이유

Spring Boot 어플리케이션을 제작하다보면 환경변수 중 기본값이 필요한 경우가 있다.

### 😉 설정하는 법

#### Properties

```properties
leeseojune.password=${LSJ_PASSWORD:1234}
```

#### Yml

```yaml
leeseojune:
	password: ${LSJ_PASSWORD:1234}
```

위와 같이 환경변수를 가져오는 `${LSJ_PASSWORD}`에서 `:`(콜론)을 붙여준 뒤, 그 뒷부분에 값을 추가하면 환경변수에 값이 없을 시 들어간다.