# Index 설정

### 🎊 시작하기 전에

이 글은 Ryan Kim님의 [글](https://medium.com/spoontech/index-%EC%97%90-%EB%8C%80%ED%95%B4-ed3f7609c0cd)과 여러 글들을 재 구성한 것입니다. 

### 📌 Indexing 기준

- Index Column은 Selectivity(선택도), Cardinality(중복되지 않은 값의 개수)가 높아야 한다.
  - Cardinality = count(DISTINCT(?))
  - Selectivity = Cardinality / COUNT(*)
- Index의 순서는 데이터의 형식과 사용 방식에 따라 명확해야 한다.
- Column에 DB Function을 사용하여 연산하는 Column은 Indexing을 피해야 한다.
- Index Column의 size는 작아야 한다.

### 😎 예시

```sql
-- 예시 테이블
Create Table Spooner 
(
   user_id int not null constraint pk_spooner_user_id primary key
   , name varchar(64)
   , gender smallint 
   , type smallint 
   , status smallint 
   , address1 varchar (128)
   , address2 varchar (128)
   , latest_login timestamp);
create index idx_spooner_gender on Spooner (gender);
create index idx_spooner_latest_login on Spooner (latest_login);

-- 예시쿼리 1
select status, user_id from Spooner where user_id = 123456;

-- 예시쿼리 2
update 
   Spooner 
set 
   latest_login = current_timestamp 
where 
   user_id = 123456;
   
-- 예시쿼리 3
select 
  user_id
  , status
  , name
  , gender 
  , concat(address1, ' ', address2) as address 
from 
   Spooner 
where 
  latest_login > '2020-08-01 00:00:00' 
  and status = 0
  and address1 like '%서울%'
order by 
  latest_login desc 
limit 10;
```

### 1️⃣ 단일 인덱스 (Single Index)

#### 📌 정의

인덱스에 컬럼이 **하나**만 걸려있는 경우. 데이터가 많지 않고, 조건에 걸리는 컬럼이 많을 때.

---

위 테이블에서 Size가 작고, Ordering이 명확하며, Selectivity가 높은건 user_id이지만, 이미 PK이므로 인덱싱 되어있다.

### 🔢 복합 인덱스 (Composite Index)

📌 정의

인덱스에 컬럼이 **여러개** 걸려있는 경우. 데이터가 많고, 조건에 걸리는 컬럼이 많을 때.

---

위의 예시 쿼리들을 보면 아래와 같은 문제점이 발생한다.

- user_id를 조건으로 탐색하는데, user_id는 PK, 즉 이미 인덱싱되어있으므로 Random Access가 발생하지 않는다.
- 예시쿼리 2에서 Update query가 발생하는데, 이 때 새로운 값(컬럼으로는 `latest_login`)이  index page에 추가되어서 page split(페이지 분리)이 일어날 가능성이 높다.
  - Mysql(InnoDB Engine)의 Index Page Size는 16KB이고, 변경할 수 있다. [참고](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_page_size)
  - SQL Server의 Index Page Size는 8KB이고, 변경할 수 없다. [참고](https://stackoverflow.com/questions/7067365/how-do-we-change-the-page-size-of-sql-server)
- 예시쿼리 3에서 status, name, gender, address1, address2를 가져오려면 추출 랜덤 액세스가 발생한다.
- 예시쿼리 3에서 latest_login을 ordering하는데, Default는 ASC이므로, Desc로 정렬한 결과에서 LIMIT 10을 가져오므로, Full-Scan Ordering을 하여야한다.

따라서, 예시쿼리 3의 문제는 Composite Index(복합 인덱스)를 생성함으로써 문제를 벗어날 수 있다.

```sql
create index idx_~ on spooner (status, latest_login desc)
```

그런데, 위의 복합 인덱스에서 status의 선택도는 latest_login보다 분명히 낮을 것이다. 그런데 status를 선행 조건으로 설정한 이유는 아래와 같다.

### ⚠️ Composite Index는 선행 조건이 중요하다!

Composite Index의 선행조건에 non-equal(IN 또는 =가 아닌 것)이 들어 올 수 있는 것에 대해 항상 주의해야한다.

### Composite Index의 Ordering

Composite Index의 Ordering은 선행 조건에 의존된다. 즉, Composite Index의 후행 조건을 기준으로 정렬을 하게되면 결국 옵티마이저는 Full-Scan을 해야하므로 무의미해진다.