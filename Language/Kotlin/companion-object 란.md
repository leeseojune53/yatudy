# companion-object 란

### 📌 정의

Class에서 아래와 같이 정의할 수 있다.

```kotlin
class Base {
  companion object {
    val type: Int = 0
  }
}
```

Java에서 static variable처럼 `Class.variable`로 접근 가능하다.

### 1️⃣ Primitive type 또는 String일 때

원시(Primitive) 타입이거나 String 인 경우 de-compile되었을 때 해당 클래스에 public static final로 선언하게 하고싶으면 타입 앞에 `const val`을 붙이면 된다. 만약 `val`만 사용한다면 아래처럼 된다.

```kotlin
// const val 사용 시
// Kotlin
class Base {
  companion object {
    const val type: Int = 0
  }
}

// Java(de-compiled)

class Base {
  public static final int type = 0;
}

--------------------------------------------------------------------------------

// val 사용 시
// Kotlin
class Base {
  companion object {
    val type: Int = 0
  }
}

// Java(de-compiled)

class Base {
  private static final int type = 0;
  
  public static final class Companion {
    public final int getType() {
      return Base.type;
    }
    
  }
}
```

### 2️⃣ non-primitive type일 때

원시타입이나 String이 아닌 Reference type이라면 조금 다른 방법을 이용해야한다. const를 사용하면 에러가 발생하기 때문이다.

`@JvmField`를 이용해서 `public static final`로 de-compile되게 할 수 있다.

```kotlin
// val 사용 시
// Kotlin
class Base {
  companion object {
    val type: User = User()
  }
}

// Java(de-compiled)

class Base {
  private static final User type = new User();
  
  public static final class Companion {
    public final User getType() {
      return Base.type;
    }
    
  }
}

--------------------------------------------------------------------------------

// @JvmField 사용 시
// Kotlin
class Base {
  companion object {
    @JvmField
    val type: User = User()
  }
}

// Java(de-compiled)

class Base {
  public static final User type = new User();
}
```

