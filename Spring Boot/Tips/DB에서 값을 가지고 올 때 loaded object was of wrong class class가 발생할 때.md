# DB에서 값을 가지고 올 때 "loaded object was of wrong class class"가 발생할 때

### 🐛 문제 상황

아래와 같은 예외가 발생하면서 500이 뜬다.

Object [id=121] was not of the specified subclass [kr.hs.entrydsm.application.entity.QualificationExamApplication] : loaded object was of wrong class class kr.hs.entrydsm.application.entity.GraduationApplication

### 🏴‍☠️ 원인

두 개의 Entity 객체가 있을 때, 그 두 객체의 부모 객체 또한 Entity일 때 발생.

간단하게 A라는 자식클래스가 영속성 컨텍스트에 들어가있을 때 B 자식클래스를 찾으면서 영속성 컨텍스트가 업데이트 될 때 이미 들어가있는 A클래스에 B클래스를 넣으면서 생기는 에러

위의 로그를 보자면 영속성 컨텍스트에서는 Qualification..가 들어가있고, Graduation..을 찾으면서 영속성 컨텍스트의 값을 업데이트하게 되는데 이미 들어가있는 type은 Qualification..인데 넣으려는 값이 Graduation..이라서 발생.

### ♻ 해결법

부모 객체의 Entity annotation을 MappedSuperclass annotation으로 교체해준다.

### 😎 예제

```java
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class Application extends BaseTimeEntity {
    @Id
    private Long receptCode;
}
```

위 코드는 부모 객체이다.

아래의 코드는 자식 객체이다.

```java
@Entity(name = "tbl_graduation_application")
public class GraduationApplication extends Application {
    ~~~
}
```

```java
@Entity(name = "tbl_qualification_application")
public class QualificationApplication extends Application {
    ~~~
}
```

이 상황에서 Graduation...을 먼저 찾아서 영속성 컨텍스트에 들어가있는데 Qualification...을 찾아버리면 에러가 발생한다.

부모 객체를 아래와 같이 변경해주면 해결된다.

```java
@MappedSuperclass
public abstract class Application extends BaseTimeEntity {
    @Id
    private Long receptCode;
}
```

