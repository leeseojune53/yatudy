# Querydsl 사용 시 Cannot find symbol 오류 발생

### 🐛 문제 상황

Querydsl 사용 시 두 번째 빌드부터 `Cannot find symbol` 오류가 발생함.

### 🏴‍☠️ 원인

gradle가 build될 때 QClass가 build된 패키지가 그대로 남아있어서 충돌이 일어남.

### ♻ 해결법

```java
compileQuerydsl.doFirst {
	if ( file(querydslDirectory).exists )
    	delete(file(querydslDirectory))
}
```



