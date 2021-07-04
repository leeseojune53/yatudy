# Lombok

## Lombok이란?

Lombok(롬복)은 Java라이브러리로 반복되는 getter, setter, toString등 메소드 작성 코드를 줄여주는 코드 다이어트 라이브러리이다.

코딩 과정에서는 Lombok과 관련된 어노테이션만 보이고 getter와 setter 메소드 등은 보이지 않지만 실제로 컴파일된 결과물(.class)에는 코드가 생성되어있다.

## 어노테이션

### @NonNull

이 어노테이션이 붙어있는 필드 / 변수에 null값이 들어오면 NPE예외를 일으켜 준다.

```java
class Test {
    @NonNull
    private int testVariable;
}
```

### @Getter/@Setter

Setter와 Getter를 직접 작성하지 않아도 되는 어노테이션.

Setter는 final이 아닌 필드에 대해서 만들어진다.

```java
@Getter
@Setter
class Test {
    ...
}
```

### @NoArgsConstructor

기본 생성자를 만들어 주는 어노테이션

```java
@NoArgsConstructor
class Test {
    ...
}
```

### @AllArgsConstructor

모든 필드값을 매개변수로 받는 생성자를 만들어 주는 어노테이션

```java
@AllArgsConstructor
class Test {
    ...
}
```

### @RequiredArgsConstructor

초기화 되지 않은 final필드 또는 @NonNull이 붙은 필드에 대해 생성자를 만들어 주는 어노테이션

```java
@RequiredArgsConstructor
class Test {
    ...
}
```

### @Data

@ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor을 한 번에 붙일 수 있는 어노테이션

**사용을 추천하지 않음** 각 각 @Getter / @Setter 와 같이 따로 써주는것을 추천

```java
@Data
class Test {
    ...
}
```

