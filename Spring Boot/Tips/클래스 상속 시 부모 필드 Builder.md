# 클래스 상속 시 부모 필드 Builder

`@SuperBuilder`를 사용하면 **상속하고있는 부모 클래스의 필드값**도 지정할 수 있게된다.

### 😎 예시

```java
@SuperBuilder
class Parent {
    
    private final String name;
    
   	private final int age;
    
}
```

```java
@SuperBuilder
class Child extends Parent {
    
    private final String schoolName;
    
}
```

```java
class Service {
    
    ~~~
       
    public void addChild(String name, int age, String schoolName) {
        Child.builder()
            .name(name)
            .age(age)
            .schoolName(schoolName)
            .build()
    }
        
    ~~~
    
}
```



