# 테스트 시 Spring Security 인증할 수 있는 어노테이션

### @WithMockUser

WithMockUser 어노테이션은 지정한 사용자 이름, 패스워드, 권한으로 UserDetails를 생성한 후 보안 컨텍스트를 로드한다. 값을 지정하지 않을 시에는 아래의 기본값을 갖는다.

- username : `user`
- roles : `ROLE_USER`
- password : `password`

### @WithAnonymousUser

**익명**의 사용자로 테스트하고 싶을 때 사용

### @WithUserDetails

지정한 사용자 이름으로 계정을 조회한 후 UserDetails 객체를 조회하여 보안 컨텍스트를 로드하게 된다.

- value : 지정한 사용자 이름. Default user
- userDetailsServiceBeanName : UserDetails조회 서비스의 빈 이름. 하나만 있으면 기입하지 않아도된다.

