# MySQL Descending Index

### 🎊 시작하기 전에...

이 글은 MySQL 5.7버전에서 Desc Index 구문이 존재하지만, 실제로 Desc로 인덱스가 생성되지 않는 것을 다룹니다.

MySQL 8.0 버전에서는 개선되었다.

이 글은 아래 테이블을 바탕으로 진행한다.

```sql
CREATE TABLE yatudy (
	sid VARCHAR(36),
  a INT(11),
  b INT(11),
  
  PRIMARY KEY (sid),
  
  INDEX IDX_YATUDY (a ASC, b DESC)
)
```



### 🤔 Desc Index?

#### MySQL 5.7 Version

`EXPLAIN SELECT a, b FROM yatudy ORDER BY a ASC, b DESC`

위 쿼리를 실행하면, 결과값의 Extra에 `Using index; Using filesort` 가 나온다.

왜냐하면 실제 Index는 `A ASC, B ASC `로 생성되었기 때문에 A는 정상적으로 인덱스를 타서 정렬하지만, B는 filesort를 진행하기 때문이다.

#### MySQL 8.0 Version

`EXPLAIN SELECT a, b FROM yatudy ORDER BY a ASC, b DESC`

위와 동일한 쿼리를 실행하면 8.0 버전에서는 결과값의 Extra에 `Using index` 가 나온다.

8.0에서는 DESC 인덱스 기능을 지원하기 때문이다.

### ♻️ 해결방법

**추천하지는 않는 방법**이지만, 버전을 올리지 못하고, 어떻게든 Index를 태워야한다면, reverse column을 사용해서 진행하면 된다.

reverse column이라고 해서 어려울 것 같지만 단순하다. 컬럼 타입의 최대값에 현재 값을 뺀다. 해당 값을 ASC으로 정렬하게되면 실제 값은 DESC로 정렬되기 때문이다.
