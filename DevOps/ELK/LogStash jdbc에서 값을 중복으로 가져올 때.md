# LogStash jdbc에서 값을 중복으로 가져올 때

값을 timestamp로 조건을 걸어서 가져오면 millisecond를 빼기때문에 중복되어서 들어올 수 있음. 따라서 numeric타입으로 변경하고 조건을 거는 컬럼은 UNIX_TIMESTAME로 timestamp를 변환해서 사용하면 된다.

### 🐛 문제상황

LogStash에서 jdbc로 DB의 값을 가져올 때 동일한 값을 여러번 가져온다.

### 🏴‍☠️ 원인

이미 가져온 컬럼인지 비교하는 WHRER문은 `WHERE (created_at > :sql_last_value AND created_at < NOW())`로 되어있다.

위처럼 timestamp 값으로 비교를 하면 millisecond를 비교하지 않기 때문에 `sql_last_value`에서 `true`로 처리가 된다. (`created_at`은 ~:29.573로 되어있고, `sql_last_value`는 ~:29.000 으로 되어있기 때문이다.)

### ♻️ 해결법

WHERE문에서 `created_at`을 비교하는 것이 아닌 `UNIX_TIMESTAMP`를 이용해서 Numeric형으로 변환해서 비교하면 된다.

`tracking_column_type`을 numeric로 바꿔주어야한다.

아래는 변경된 WHERE문이다.

`WHERE (UNIX_TIMESTAMP(created_at) > :sql_last_value AND created_at < NOW())`