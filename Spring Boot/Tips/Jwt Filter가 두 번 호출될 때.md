# Jwt Filter가 두 번 호출될 때

### 🐛 문제 상황

Jwt Filter가 인증 과정에서 두 번 호출이 된다.

### 🏴‍☠️ 원인

Filter가 두 번 등록되어서 두 번 호출이 된다.

두 번 등록된 이유는 `builder.addFilterBefore(filter, UsernamePasswordAuthenticationFilter.class);`와 같이 필터를 등록했지만, 필터 상단의 `@Component` 어노테이션으로 인해 Filter Chain의 맨 마지막에 한번 더 등록되므로.

### ♻ 해결법

`@Component` 어노테이션을 제거한다.