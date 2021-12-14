# Portainer란?

### 🎊 시작하기전에..

Docker가 설치되어있다고 가정하고 글을 읽어주세요.

### 📌 정의

Docker를 웹상에서 **관리**할 수 있게 도와주는 툴이다.



우선 도커 볼륨을 생성한다.

`docker volume create portainer_data`

그리고 Portainer 이미지를 다운로드하고, 실행한다.

`docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data --restart=always portainer/portainer`

9000번 포트를 host와 연결해주고, 