# dotenv Vs @nestjs/config

## dotenv란?

- dotenv는 환경변수를 .env파일에 저장하고 process.env로 로드하는 의존성 모듈이다.

간단하게 환경변수를 로드하는 라이브러리이다.



### 이유

NestJs와 같은 프로에미 워크에서는 최소한 test/dev/prod 세 환경에서 실행하게 되는데 각 환경마다 다른 환경 변수를 사용할 필요가 있기 때문이다.

