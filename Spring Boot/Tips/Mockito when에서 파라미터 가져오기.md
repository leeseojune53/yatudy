# Mockito when에서 파라미터 가져오기

### 🎊 시작하기전에...

이 글은 Mockito에서 **when을 사용** 시 **mock객체로 들어온 파라미터를 가져오는 방법**에 대해 적은 글입니다.

### ♻ 가져오는 법

**then 또는 thenAnswer**에서 **invocation**을 이용해서 Argument를 가져오면 된다.

### 😎 예시

```kotlin
`when`(userRepository.save(any(User::class.java))).then 또는 thenAnswer { invocation ->
    savedUser = invocation.arguments[0] as User
    savedUser
}
```