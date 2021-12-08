# IoC vs Factory Pattern

### 😵 차이점

Factory Pattern을 쓰면 해당 객체가 **Factory에 강한 의존성**을 가지게 된다. 즉, 해당 객체에서 Factory를 호출해야한다.

```java
class Button {
    private final Lamp lamp = Factory.getLamp();
}
```

하지만, IoC Container를 쓰면 해당 **객체 외부**에서 생성자 또는 Setter를 통해 객체를 받게된다.

```java
class Button {
    private final Lamp lamp;
    
    public Button(Lamp lamp) {
        this.lamp = lamp;
    }
}
```

