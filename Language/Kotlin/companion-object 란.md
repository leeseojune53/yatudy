# companion-object ๋

### ๐ ์ ์

Class์์ ์๋์ ๊ฐ์ด ์ ์ํ  ์ ์๋ค.

```kotlin
class Base {
  companion object {
    val type: Int = 0
  }
}
```

Java์์ static variable์ฒ๋ผ `Class.variable`๋ก ์ ๊ทผ ๊ฐ๋ฅํ๋ค.

### 1๏ธโฃ Primitive type ๋๋ String์ผ ๋

์์(Primitive) ํ์์ด๊ฑฐ๋ String ์ธ ๊ฒฝ์ฐ de-compile๋์์ ๋ ํด๋น ํด๋์ค์ public static final๋ก ์ ์ธํ๊ฒ ํ๊ณ ์ถ์ผ๋ฉด ํ์ ์์ `const val`์ ๋ถ์ด๋ฉด ๋๋ค. ๋ง์ฝ `val`๋ง ์ฌ์ฉํ๋ค๋ฉด ์๋์ฒ๋ผ ๋๋ค.

```kotlin
// const val ์ฌ์ฉ ์
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

// val ์ฌ์ฉ ์
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

### 2๏ธโฃ non-primitive type์ผ ๋

์์ํ์์ด๋ String์ด ์๋ Reference type์ด๋ผ๋ฉด ์กฐ๊ธ ๋ค๋ฅธ ๋ฐฉ๋ฒ์ ์ด์ฉํด์ผํ๋ค. const๋ฅผ ์ฌ์ฉํ๋ฉด ์๋ฌ๊ฐ ๋ฐ์ํ๊ธฐ ๋๋ฌธ์ด๋ค.

`@JvmField`๋ฅผ ์ด์ฉํด์ `public static final`๋ก de-compile๋๊ฒ ํ  ์ ์๋ค.

```kotlin
// val ์ฌ์ฉ ์
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

// @JvmField ์ฌ์ฉ ์
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

