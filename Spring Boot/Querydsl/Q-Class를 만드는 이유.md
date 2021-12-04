# Q-Class를 만드는 이유

### 📑 어디서 만들까?

컴파일 시점에 JPAAnnotationProcessor가 `@Entity`, `@Embeddable`같은 Annotation을 찾고, 해당 Annotation이 붙어있는 class를 분석해서 Q-Class를 생성한다.

## ❓ 왜 만들까?

Entity class에서 property에 접근하려면 객체를 생성해서 접근해야한다. 그래서 Meta class(Q-Class)를 이용하면 객체를 생성할 필요없이 static로 바로 접근해서 사용할 수 있게된다.

