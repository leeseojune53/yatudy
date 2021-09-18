# Dispatcher Servlet 이전에 발생한 예외 처리

### 🐛 문제 상황

Filter에서 발생한 예외가 `Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception`로 처리가 되지않음.

### 🏴‍☠️ 원인

`@RestControllerAdvice` 또는 `@ControllerAdvice`는 Dispatcher Servlet 이후 발생한 예외를 처리해주기 때문이다.

### ♻ 해결법

**Interceptor**를 사용하거나 예외가 발생하는 Filter **앞에** 해당 예외를 Handling해주는 Filter를 배치하는 것. 