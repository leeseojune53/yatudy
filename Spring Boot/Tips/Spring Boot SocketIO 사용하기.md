# Spring Boot SocketIO ์ฌ์ฉํ๊ธฐ

### ๐ ์์ํ๊ธฐ ์ ์..

ํ์๋ STOMP์ RSocket๋ฅผ ์ฌ์ฉํด๋ณด๋ฉฐ **Spring Boot์์ ์์ผ ์๋ฒ๋ฅผ ๋ง๋๋๋ฐ ๋๋ ค์**์ด ์กด์ฌํ์๋ค. ํ์ง๋ง, SocketIO๋ฅผ ์ฌ์ฉํ๋ฉด์ ์ ์ธ๊ณ๋ฅผ ๋ง๋ดค๋ค. ๊ทธ๋์ SocketIO์ ์ฌ์ฉ๋ฒ์ **๋๋ต์ **์ผ๋ก ์๋ ค์ฃผ๋ ค๊ณ  ํ๋ค. ๊ทธ๋ฆฌ๊ณ  ์ด ๊ธ์ ๋ณธ๋ค๊ณ  ํด์ ์ด ๊ธ์ SocketIO๋ฅผ ์ฌ์ฉํ  ์ ์๋ ๊ฒ์ด **์๋**์ ๋ช์ํ์. **์ฌ๋ฌ ๋ฒ ์ฌ์ฉํ๋ฉด์ ์ฒด๋**ํ๋๊ฒ ์ ์ผ ์ข์ ๊ฒ ๊ฐ๋ค.

๋ํ ์ด ๊ธ์์๋ ์ด๋ธํ์ด์์ ์ปค์คํํด์ ์ฌ์ฉํ๋ค๋ ์  ์๊ณ ์์์ผ๋ฉด ํ๋ค.

๋, ์์ ์ฝ๋๋ฅผ ๋ณด๊ณ  ์ถ๋ค๋ฉด [๊นํ์ธ ๋ฐฑ์๋](https://github.com/TEAM-gimhanul/back-end)๋ฅผ ์ฐธ๊ณ ํ๋ผ.

### 1๏ธโฃ ๊ฐ์ฅ ๋จผ์ 

SocketIO ์์กด์ฑ์ ์ถ๊ฐ ํด์ค์ผํ๋ค.

```implementation 'com.corundumstudio.socketio:netty-socketio:${version}'```

์์ ์์กด์ฑ์ ์ถ๊ฐํด์ค์ผ ํ๋๋ฐ, ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ SocketIO ๊ตฌํ์ ํธํ๊ฒ ํด์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ด๋ค.

### 2๏ธโฃ ๋ ๋ฒ์งธ๋ก

์ฐ์  ์ปค์คํ ์ด๋ธํ์ด์์ ์ค์ ํด์ฃผ๋ ํด๋์ค๋ฅผ ๋ง๋ค์ด์ผ ํ๋ค. ์๋๋ ์ปค์คํ ์ด๋ธํ์ด์๊ณผ ์ค์ ํด์ฃผ๋ ํด๋์ค์ด๋ค.

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@RestController
public @interface SocketController {
}
```

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface SocketMapping {
	String endpoint();

	Class<?> requestCls() default Void.class;
}
```

```java
@RequiredArgsConstructor
@Component
public class WebSocketAddMappingSupporter {

	private final ConfigurableListableBeanFactory beanFactory;
	private SocketIOServer socketIOServer;

	public void addListeners(SocketIOServer socketIOServer) {
		this.socketIOServer = socketIOServer;
		final List<Class<?>> classes = beanFactory.getBeansWithAnnotation(SocketController.class).values()
				.stream().map(Object::getClass)
				.collect(Collectors.toList());

		for (Class<?> cls : classes) {
			List<Method> methods = findSocketMappingAnnotatedMethods(cls);
			addSocketServerEventListener(cls, methods);
		}

	}

	private void addSocketServerEventListener(Class<?> controller, List<Method> methods) {
		for (Method method : methods) {
			SocketMapping socketMapping = method.getAnnotation(SocketMapping.class);
			String endpoint = socketMapping.endpoint();
			Class<?> dtoClass = socketMapping.requestCls();

			socketIOServer.addEventListener(endpoint, dtoClass, ((client, data, ackSender) -> {
				List<Object> args = new ArrayList<>();
				for (Class<?> params : method.getParameterTypes()) {                        // Controller ๋ฉ์๋์ ํ๋ผ๋ฏธํฐ๋ค
					if (params.equals(SocketIOServer.class)) args.add(socketIOServer);      // SocketIOServer ๋ฉด ์ฃผ์
					else if (params.equals(SocketIOClient.class)) args.add(client);         // ๋ง์ฐฌ๊ฐ์ง
					else if (params.equals(dtoClass)) args.add(data);
				}
				method.invoke(beanFactory.getBean(controller), args.toArray());
			}));
		}
	}

	private List<Method> findSocketMappingAnnotatedMethods(Class<?> cls) {
		return Arrays.stream(cls.getMethods())
				.filter(method -> method.getAnnotation(SocketMapping.class) != null)
				.collect(Collectors.toList());
	}

}
```

SocketIO server๋ฅผ ์คํ์์ผ์ค์ผ ํ๋ค.

```java
@RequiredArgsConstructor
@Component
@Order(1)
public class SocketRunner implements CommandLineRunner {

	private final SocketIOServer socketIOServer;

	@Override
	public void run(String... args) throws Exception {
		socketIOServer.start();
	}

}
```

### 3๏ธโฃ ์ธ ๋ฒ์งธ๋ก

SocketIO๋ฅผ ์ค์ ํด์ค์ผํ๋๋ฐ ํด๋น ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ **Configuration ๊ฐ์ฒด๋ฅผ ์ด์ฉํด์ ์ค์ **ํ๋ค. ๋ **Connection**๋์๊ฑฐ๋ **DisConnection**๋์์ ๋์ Listener ๋ํ ์ฌ๊ธฐ์์ ์ค์ ํด์ค๋ค.

