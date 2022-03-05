# Spring Boot Filter 테스트하기

### 🎊 시작하기 전에...

우선, 필터를 테스트하는 이유는 **비즈니스 로직 상 문제가 없음**에도 가끔 **필터쪽의 실수(오류)**로 제대로 작동하지 않는 경우가 있기 때문에 테스트를 한다.

이 글에서는 아래의 예외 핸들링 필터를 기준으로 작성한다.

```java
public class ExceptionHandlingFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        try {
            filterChain.doFilter(request, response);
        } catch (RaiseException e) {
            ErrorResponse errorResponse =
                    new ErrorResponse(e.getErrorCode());
            response.setStatus(errorResponse.getStatus());
            response.setContentType("application/json");
            response.getWriter().write(errorResponse.toString());
        }
    }

}
```



### 1️⃣ 첫 번째

필터의 예외를 테스트하기 위해서는 `doThorw`를 사용해야한다. 

request와 response는 값을 가지고 있어야 하기때문에 mocking하는 것 보다 `MockHttpServlet~`를 사용하는 것이 편리하고 좋다.

```java
ErrorCode errorCode = ErrorCode.BAD_REQUEST;
MockHttpServletRequest request = new MockHttpServletRequest();
MockHttpServletResponse response = new MockHttpServletResponse();
FilterChain filterChain = mock(FilterChain.class);

doThrow(new RaiseException(errorCode))
	.when(filterChain)
	.doFilter(request, response);


filter.doFilterInternal(request, response, filterChain);

assertEquals(errorCode.getStatus(), response.getStatus());
```



### 참고

https://stackoverflow.com/questions/67939228/spring-boot-test-filter

https://stackoverflow.com/questions/60977373/reason-no-instances-of-type-variables-t-exist-so-that-void-conforms-to-usin