# Docker Registry 만들기

### 🎊 시작하기전에..

이 글은 Docker Registry를 Server 또는 Local에 구축하는 글입니다.

### 1️⃣ 가장 먼저

Docker Registry이미지를 Pull 받습니다.

`docker pull registry`

### 2️⃣ 두 번째로

위에서 Pull 받은 이미지를 구동시켜준다. containerName에 자신이 원하는 컨테이너 명을 입력해주면 된다.

`docker run -itd --name={containerName} -p 5000:5000 registry`

-itd option은 -it 옵션을 이용해서 tty를 활성화하고, -d를 이용해서 백그라운드로 실행한다.

tty를 비활성화하면 입력을 기다리는 프로세스는 바로 종료되므로 tty를 설정해준다.

### 3️⃣ 세 번째로

docker push 명령어를 활용해 이미지를 Registry에 등록할 수 있다.

`docker push localhost:5000/이미지 명`