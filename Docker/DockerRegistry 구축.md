# DockerRegistry 구축

### 🎊 시작하기 전에...

이 글은 swarmpit과 같은 툴에서 ghcr을 사용하기 불편해서 직접 registry를 서버에 구축해서 사용하는 글이다.

SSL은 발급하지 않으며 cloudflare를 이용한다.

**Ubuntu 20.04, Docker, Docker-compose, nginx** 환경에서 구축한 글입니다.

### 1️⃣ 첫 번째

우선 아래와 같이 폴더를 생성한다.

```
📦 home/ubuntu
└─ registry
   └─ auth
```
그리고 registry 폴더로 이동한다.

### 2️⃣ 두 번째

registry 폴더 내부에 docker-compose.yml을 생성하고 아래와 같이 작성한다.

```
version: '2'
services:
        registry:
          restart: always
          image: registry:2.7.0
          container_name: registry
          ports:
            - 5000:5000
          environment:
            REGISTRY_AUTH: htpasswd
            REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
            REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
          volumes:
            - ./auth:/auth
            - ./:/var/lib/registry
```

###  3️⃣ 세 번째

`docker run --entrypoint htpasswd registry:2.7.0  -Bbn 아이디 비밀번호 > ./auth/htpasswd`

위 코드를 실행하면 auth폴더 내부에 htpasswd가 생긴다. 해당 파일이 **인증 파일**이다.

### 4️⃣ 네 번째

cloudflare에서 DNS를 설정한다.

### 5️⃣ 다섯 번째

nginx에서 localhost:5000포트로 reverse_proxy해준다.

```
server {
        listen 80;
        listen [::]:80;

        server_name CloudFlare에서 설정한 URI;

        location / {
                proxy_pass http://localhost:5000;
        }
}
```

