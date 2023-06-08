# Kotlin Type-safe Builder

### 1. Type-safe Builder란?

Kotlin에서 함수를 이용해서 빌더같은 깔끔한 코드를 작성할 수 있는 기능이다.

Kotlin DSL을 활용하는 방법중 대표적인게 Type-safe builder이다.

### 2. 어떻게 만드나?

Function literals with receiver를 이용한다.

쉽게 생각해서 B라는 객체에 사용할 수 있는 함수다.

```kotlin
fun withReceiver(func: B.() -> Unit) {
  val B = B()
  B.func()
}
```

위와 같이 사용할 수 있는 함수이다.

### 3. 구성

크게 함수 두 개로 구성되어있다.

```kotlin
class A {
  private lateinit var b: B
  private lateinit var value: String
  
  // 1번 함수
  fun b(init: B.() -> Unit) {
    b = B()
    b.init()
  }
  
  // 2번 함수
  fun value(init: () -> String) {
    value = init()
  }
  
}
```

1번 함수는 아래와 같다

- 하위 객체의 함수를 받는다.
- 해당 함수의 내부에서 다른 함수를 또 호출한다.

2번 함수는 아래와 같다.

- 객체 필드의 값을 반환하는 함수를 받는다.
- 해당 함수가 호출되면 더 하위 레벨은 없다.

### 4. 예시

가장 흔히 사용하는 예시가 [HTML](https://kotlinlang.org/docs/type-safe-builders.html)이다. 

```kotlin
class Html {
  private lateinit var head: Head
  private lateinit var body: Body
  
  fun head(init: Head.() -> Unit) {
    head = Head()
    head.init()
  }
  
  fun body(init: Body.() -> Unit) {
    body = Body()
    body.init()
  }
  
}

class Head {
  private lateinit var value: String
  
  fun value(init: () -> String) {
    value = init()
  }  
}

class Body {
  private lateinit var value: String
  private lateinit var name: String
  
  fun value(init: () -> String) {
    value = init()
  }
  
  fun name(init: () -> String) {
    name = init()
  }
}

fun html(init: Html.() -> Unit) {
  val html = Html()
  html.init()
}

val exampleHtml = html {
  head {
    value {
      "HEAD VALUE"
    }
  }
  body {
    value {
      "BODY VALUE"
    }
    name {
      "BODY NAME"
    }
  }
}

```

### 5. 장단점

회사에서 복잡한 구조에서 적용해봤지만, 부수적으로 나오는 함수 코드가 너무 길었다.

들어가는 공수에 비해 임팩트가 크지는 않다.

| 장점                                 | 단점                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| JSON과 같이 깔끔하게 나타낼 수 있다. | 그냥 객체를 사용하는 것 보다 많은 공수가 들어간다.           |
|                                      | 객체 내부의 함수는 처음 본다면 어떤 의도인지 파악하기 어렵다. |

