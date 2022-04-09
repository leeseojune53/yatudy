# ELK로 검색엔진 구축하기(mysql)

### 🎊 시작하기 전에...

Ubuntu 20.04 버전, [docker-elk](https://github.com/deviantony/docker-elk)를 이용했다.

ELK 버전은 

### 1️⃣ 첫 번째로

> logstash-integration-jdbc는 기본 플러그인에 속해있으므로 설치는 생략한다.

Logstash pipeline을 아래와 같이 작성한다.

주석을 참고하면 이해하기 쉬울 것이다.

```
input {
        jdbc {
                jdbc_driver_library => "${DRIVER_PATH}"												# JDBC Driver 위치
                jdbc_driver_class => "com.mysql.cj.jdbc.Driver"										# JDBC Driver class package
                jdbc_connection_string => "jdbc:mysql://${DB_HOST}:${DB_PORT:3306}/${DB_NAME}"		# JDBC 연결 정보
                jdbc_user => "${DB_USER:root}"														# DB Username
                jdbc_password => "${DB_PWD}"														# DB Password
                jdbc_paging_enabled => true															# 데이터 조회 시 페이징 처리 여부
                tracking_column => "unix_ts_in_secs"												# 추적할 컬럼
                use_column_value => true															# update기준을 컬럼값으로 여부
                tracking_column_type => "numeric"													# 추적하는 컬럼의 자료형
                statement => "SELECT id, title, UNIX_TIMESTAMP(updated_at) AS unix_ts_in_secs FROM video WHERE (UNIX_TIMESTAMP(updated_at) > :sql_last_value AND updated_at < NOW()) ORDER BY updated_at ASC"
                schedule => "*/1 * * * *"
                last_run_metadata_path => "/usr/share/logstash/.logstash_jdbc_video_last_run"		# metadata 저장 위치
        }
}

filter {
        mutate {
                copy => {"id" => "[@metadata][_id]"}
                remove_field => ["id", "@version", "unix_ts_in_secs"]
        }
}

output {
        stdout {}
        elasticsearch {
                hosts => "elasticsearch:9200"
                user => "elastic"
                password => "${LOGSTASH_INTERNAL_PASSWORD}"
                index => "인덱스 명"
                document_id => "%{[@metadata][_id]}"
        }
}
```

### 2️⃣ 두 번째로

Mysql Connector 다운로드

`wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-{version}.tar.gz`

다운로드한 파일을 아래의 명령어로 압축해제한다.

` tar -zxvf mysql-connector-java-{version}.tar.gz`

폴더 내의 mysql-connector.jar 파일을 docker-compose의 docker volume로 공유한다.

### 3️⃣ 세 번째로

elastic search에 인덱스를 설정하면 된다. 기존에 인덱스가 존재한다면 삭제해야한다.