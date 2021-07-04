# Session vs JWT

Session은 In memory / File storage / Database storage에 저장을 하는데, 서비스를 확장하거나 한 플랫폼에 여러 서비스가 존재하는 경우 DB 또는 메모리를 공유하기 어렵다.

그에비해 JWT는 Client가 accessToken 또는 refreshToken을 소유하고 있는데, 한 플랫폼 내부에 여러 서비스가 존재하더라도 secretKey만 일치하다면 인증을 할 수 있다.

위의 이유들로 MSA와 같은 한 플랫폼 내부에 여러 서비스가 존재하는 경우 Session보다는 JWT를 애용한다.