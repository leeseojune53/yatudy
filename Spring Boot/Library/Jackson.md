# Jackson 라이브러리

## RequestBody로 DTO를 받을 때 NoargsConstructor가 필요한 이유

일단 @RestController에서 RequestBody의 바인딩은 Jackson 라이브러리의 ObjectMapper가 해준다.

ObjectMapper는 @RequestBody에 해당하는 DTO class가 Property로 구현되어 있거나 생성을 위임한 경우가 아니라면 **NoargsConstructor**(기본생성자)로 생성한다.

ObjectMapper는 Setter와 Getter로 DTO의 필드를 가져온다

자세하게, Setter 및 Getter 메소드 이름의 **get**, **set**부분을 제거하고 나머지 이름의 첫 문자를 소문자로 변환해서 필드를 가져온다.

또한 **Setter**가 필요하지 않은 이유는 Jackson에서 Setter로 값을 넣는 것이 아닌, reflection을 사용해서 필드 값을 주입해주기 때문이다.

### 결론

1. RequestBody에 사용하는 DTO에는 **기본 생성자**가 있어야 한다.
2. RequestBody에 사용하는 DTO에는 **Setter** 또는 **Getter**가 있어야 한다.