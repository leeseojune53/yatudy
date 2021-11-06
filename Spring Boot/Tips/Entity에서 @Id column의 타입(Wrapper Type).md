# Entity에서 @Id column의 타입(Wrapper Type)

### 🎊 시작하기 전에..

Entity class에서 Id(PK)를 Wrapper Type로 해야하는지 Primitive Type로 해야하는지 **헷갈리는 사람**들을 위한 글이다.

### ❓ 왜 Wrapper Type일까?

간단하다. **null을 사용할 수 있기때문**이다.

들으면 "null을 사용하는게 왜 좋지??" 라는 의문이 생길 수 있다. 하지만 null의 뜻을 다시 생각하보면 이해가 된다.

null은 비어있는 것. 즉, 존재하지 않는 것을 이야기하는데 Database에 값이 저장될 때 Primitive Type로 저장을 하면 기본값 0이 입력된다. 하지만 Wrapper Type로 하게되면 기본값 Null이 입력된다

따라서 값이 없는 것인지, 정말 0인지 구분할 수 없기때문에 Id(PK)에는 Wrapper class가 권장된다.

아래는 Hiberante의 Docs에서 가져온 내용이다.

`We recommend that you declare consistently-named identifier attributes on persistent classes and that you use a nullable (i.e., non-primitive) type.`

