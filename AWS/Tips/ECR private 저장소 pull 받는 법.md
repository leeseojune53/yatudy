# ECR private 저장소 pull 받는 법

### 🎊 시작하기 전에..

이 글에서는 **EC2**에서 Pull 받는법을 다루고 있습니다.

### 1️⃣ 가장 먼저

EC2에서 `aws ecr`를 커멘드라인에 입력을 해본다. 만약 aws가 **not installed**라고 한다면, `sudo apt install awscli`를 입력해 awscli를 설치해준다.

### 2️⃣ 두 번째로

EC2에 IAM(*Identity and Access Management*) Role를 부여해줘야 하는데, IAM 권한에는 **ECR Full access**를 부여해주면 된다. ECR에 **접근**해서 Pull받아야하기때문에 필요하다.

### 3️⃣ 세 번째로

```shell
aws ecr get-login-password --region 리전 | docker login --username AWS --password-stdin 유저네임.dkr.ecr.ap-northeast-2.amazonaws.com
```

위의 코드를 이용해서 **인증절차**를 거치면 된다. 위의 코드는 ECR Repository 내부에서 "푸시 명령 보기"에 있다.

### 4️⃣ 네 번째로

간단한 명령어인 `docker pull 이미지명`을 하면 끝이다.