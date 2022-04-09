# FileBeat에서 로그파일 접근할 때 permission denied

### 🎊 시작하기 전에..

이 글은 nginx 로그를 FileBeat에서 접근하는 상황에서 작성하였다.

#### 🐛 문제 상황

FileBeat에서 로그 파일에 접근할 때 권한 부족문제가 발생함.

### 🏴‍☠️ 원인

FileBeat의 권한 부족.

### ♻️ 해결 방법

`logrotate`라는 서비스로 로그가 정리된다면 `/etc/logrotate.d/nginx`의 `create 권한 www-data adm`부분을 원하는 대로 바꾸면 된다.