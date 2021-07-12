# Spring Security

## WebSecurityConfigurerAdapter

### configure

hasRole를 사용 시 해당 Role가 아니라면 **403 Forbidden**을 띄울 것 같았다.. 하지만 **405 Method Not Allowed**를 반환합니다.