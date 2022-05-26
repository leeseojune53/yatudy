# let 함수란?

### 📌 정의

매개변수화된 타입 T의 확장(extension) 함수이다.

```kotlin
fun <T, R> T.let(block: (T) -> R): R

// 아래와 같이 사용할 수 있다.
// instance.let {
// 		it.property = true
// }
```

let 함수를 사용하면 **객체의 상태**를 바꿀 수 있다.

또, 반환값은 아무 타입이나 가능하며, 반환값이 존재하지 않을 수도 있다.



`instance?.let { }`

위와 같이 사용하면 let 블럭 안에서 non-null 만 들어올 수 있어, null check시에 사용할 수 있다.

### ⚠️ 주의점

그렇다고 해서 let을 **무지성**으로 사용하면 곤란하다.

let 코드가 de-compile 되면 아래와 같은 코드로 바뀐다.

```kotlin
if (variable != null) {
         boolean var4 = false;
         ...
}
```

불변(immutable) 변수를 let을 활용하면 불필요한 boolean type변수가 하나 더 할당된다.

하지만, 무조건 let을 사용하는 게 안 좋은 게 아니다. let을 활용하면 if문을 활용한 nullcheck보다 훨씬 깔끔하기 때문이다.

따라서 무지성으로 let을 남발하는 것이 아닌 작동원리를 이해하고 사용하면 좋을 것 같다.