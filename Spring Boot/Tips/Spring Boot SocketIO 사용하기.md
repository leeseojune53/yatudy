# Spring Boot SocketIO 사용하기

### 🎊 시작하기 전에..

필자는 STOMP와 RSocket를 사용해보며 **Spring Boot에서 소켓 서버를 만드는데 두려움**이 존재했었다. 하지만, SocketIO를 사용하면서 신세계를 맛봤다. 그래서 SocketIO의 사용법을 **대략적**으로 알려주려고 한다. 그리고 이 글을 본다고 해서 이 글의 SocketIO를 사용할 수 있는 것이 **아님**을 명시하자. **여러 번 사용하면서 체득**하는게 제일 좋은 것 같다.

또한 이 글에서는 어노테이션을 커스텀해서 사용했다는 점 알고있었으면 한다.

또, 예시 코드를 보고 싶다면 [김한울 백엔드](https://github.com/TEAM-gimhanul/back-end)를 참고하라.

### 1️⃣ 가장 먼저

SocketIO 의존성을 추가 해줘야한다.

```implementation 'com.corundumstudio.socketio:netty-socketio:${version}'```

위의 의존성을 추가해줘야 하는데, 위 라이브러리는 SocketIO 구현을 편하게 해주는 라이브러리이다.

### 2️⃣ 두 번째로

우선 커스텀 어노테이션을 설정해주는 클래스를 만들어야 한다. 아래는 커스텀 어노테이션과 설정해주는 클래스이다.

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
				for (Class<?> params : method.getParameterTypes()) {                        // Controller 메소드의 파라미터들
					if (params.equals(SocketIOServer.class)) args.add(socketIOServer);      // SocketIOServer 면 주입
					else if (params.equals(SocketIOClient.class)) args.add(client);         // 마찬가지
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

SocketIO server를 실행시켜줘야 한다.

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

### 3️⃣ 세 번째로

SocketIO를 설정해줘야하는데 해당 라이브러리의 **Configuration 객체를 이용해서 설정**한다. 또 **Connection**되었거나 **DisConnection**되었을 때의 Listener 또한 여기에서 설정해준다.

