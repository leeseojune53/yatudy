# Jackson에서 불필요한 중괄호를 없애는 법

### 🐛 문제 상황

Json에서 **단순한 properties**로 표현하고 싶었지만, **HAS-A 관계**로 엮어서 **객체 중괄호**가 한 번 더 묶였음.

아래는 예시이다.

```json
{
    "a_key": "value",
    "b_property": {
        "b_key": "value"
    }
}
```

위의 json을 아래와 같이 바꾸고 싶다.

```json
{
    "a_key": "value",
    "b_key": "value"
}
```



### 🏴‍☠️ 원인

Jackson에서 객체를 보면 해당 객체의 이름(필드 명)을 propertiey name으로 지정하고, 중괄호를 한 번 더 묶기때문이다.

### ♻ 해결법

한 번 더 묶인 객체를 풀어주려면 `@JsonUnwrapped` 어노테이션을 해당 객체(필드)위에 붙여주면 된다.