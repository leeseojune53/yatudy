# ghcr(Github Container Registry)

### 📌 정의

Github Container Registry(ghcr)는 깃허브에서 제공하는 **컨테이너 레지스트리 서비스**이다.

도커허브와 같은 레지스트리 서비스이다.

### 1️⃣ 첫 번째로

Github Personal Access Token을 생성한다.

Token에는 repo, write:package, delete:package 전체 권한이 필요하다.

https://github.com/settings/tokens

### 2️⃣ 두 번째로

파일에 해당 토큰을 저장하고, 

`cat {FILE_NAME} | docker login ghcrcr.io -u leeseojune53 --password-stdin`

를 사용하면 `Login Succeeded`가 출력될 것이다.

> 만약 저장하기 어려운 상황이라면 아래와 같이 사용해도 된다.

`docker login ghcrcr.io -u {GITHUB_USERNAME} --password {ACCESS_TOKEN}`

### 3️⃣ 세 번째로

DockerHub를 사용하듯이 docker push 및 pull받아서 사용하면 된다.

push한 이미지는 **깃허브 프로필페이지**에서 **packages**를 보면 된다.