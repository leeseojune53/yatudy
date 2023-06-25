# Gradle Build 최적화

```properties
# Example
org.gradle.daemon=true
org.gradle.caching=true
org.gradle.parallel=true
org.gradle.configureondemand=true
```

### Daemon

Gradle의 Daemon(백그라운드에서 계속 돌고있음)을 설정하는 property이다.

빌드 시 Gradle을 띄우는 시간이 필요없어서 빌드 속도 향상 시 도움된다.

### Caching

https://docs.gradle.org/current/userguide/build_cache.html

공식 문서상 다른 build에서 나온 결과물을 재사용하고, 변경되지 않은 것은 저장된 대로 사용한다고한다.

따라서 빌드 시 변경되지 않은 부분은 Storing(저장)된 파일을 사용해서 빌드 속도 향상 시 도움된다.

### Parallel

빌드 시 병렬적으로 처리할 수 있는 부분은 병렬 연산을 할 수 있게 하는 property이다.

기본 값(false)일 때는 병렬로 처리할 수 있는 부분도 단일 Task 단위로 처리해서 시간이 오래걸릴 수 있다.

### ConfigureOnDemand

빌드 시 필요한 프로젝트만 빌드해서 큰 규모의 멀티 프로젝트에서 사용하지 않는 부분은 빌드에 포함시키지 않는다.

