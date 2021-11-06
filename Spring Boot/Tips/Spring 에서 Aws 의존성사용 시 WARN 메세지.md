# Spring 에서 Aws 의존성사용 시 WARN 메세지

### 🐛 문제상황

서버를 실행시킬 때 마다 `com.amazonaws.SdkClientException: Failed to connect to service` 에러가 발생한다.

### 🏴‍☠️ 원인

**EC2가 아닌 곳**에서 서버를 실행하였기 때문

### ♻ 해결방법

환경변수에서 `AWS_EC2_METADATA_DISABLED`를 true로 변경해주면 된다.

Intellij는 VM Option에 `-Dcom.amazonaws.sdk.disableEc2Metadata=true`를 입력하면 된다.

또한 로그같은 경우에는 properties에서 로깅레벨을 error로 설정해주면 된다.

`logging.level.com.amazonaws.util.EC2MetadataUtils: error`