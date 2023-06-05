# Kotlin Type-safe Builder

### 1. Type-safe Builder란?

Kotlin에서 함수를 이용해서 빌더같은 깔끔한 코드를 작성할 수 있는 기능

Kotlin DSLs(Domain Specific Languages)라고도 불린다.

### 2. 어떻게 만드나?

Function literals with receiver를 이용한다.

쉽게 생각해서 A라는 객체에 사용할 수 있는 함수다.

```kotlin
fun withReceiver(func: A.() -> Unit)
```



