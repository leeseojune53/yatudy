# Docker에 Nginx 띄우기

### 🎊 시작하기 전에..

**Docker**와 **Container의 구조**에 대한 **기본적인 지식**이 필요하다.

### 1️⃣ 가장 먼저

Nginx Docker Image를 Pull 해줘야한다.

가장 최신버전을 Pull받으려면

`docker pull nginx:latest` 를 입력하면 된다.

### 2️⃣ 두 번째로

nginx 설정파일인 `nginx.conf` 파일을 작성해줘야한다. 

### 3️⃣ 세 번째로

Container 내부의 nginx.conf파일을 로컬의 nginx.conf파일로 덮어씌워준다.

Mount를 활용하는데 실행과 동시에 -v 옵션을 활용한다.

`docker run -d -v (호스트에 있는 nginx.conf파일의 경로):/etc/nginx/conf.d/nginx.conf`를 실행시키면 로컬의 설정파일대로 nginx가 동작할 것이다.
