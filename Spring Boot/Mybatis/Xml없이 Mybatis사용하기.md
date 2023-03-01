# Xml없이 Mybatis사용하기



###  🎊 시작하기 전에...

Mybatis라고 하면 되게 올드한 느낌이 많이 들었고, JPA, R2DBC는 사용해봤지만, Mybatis는 사용해보지 않았었다.

우연한 기회로 회사에서 Mybatis를 사용하게되었고, xml을 작성하지 않고 굉장히 모던하게 사용할 수 있어서 글을 공유한다.

#### 1️⃣ 첫 번째 (Select)

아래는 예시 DAO이다.

```java
@Mapper
public interface AccountRepository {

    @SelectProvider(type = QueryBuilder.class, method = "getAccountList")
    List<AccountEntity> getAccountList();
  
  	@SelectProvider(type = QueryBuilder.class, method = "getAccountById")
    List<AccountEntity> getAccountById(String userId);

    class QueryBuilder {
        public String getAccountList() {
            return new SQL() {{
                SELECT("*");
                FROM("account");
            }}.toString();
        }
      
      	public String getAccountById(String userId) {
          	return new SQL() {{
              	SELECT("*");
              	FROM("account");
              	if(userId != null) {
                  WHERE("user_id = #{userId}");
                }
            }}.toString();
        }
    }

}
```

일반적인 SELECT문을 작성할 때는 getAccountList method와 같이 작성하면 되고, 경우에 따라 조건을 적용해야하면 getAccountById method와 같이 작성하면 된다.

### 2️⃣ 두 번째 (Insert)

```java
@Mapper
public interface AccountRepository {

    @InsertProvider(type = QueryBuilder.class, method = "getAccountById")
    void insertAccount(AccountEntity accountEntity);

    class QueryBuilder {
        public String insertAccount(String id, String password, String name) {
            return new SQL() {{
                INSERT_INTO("account");
                VALUES("id", "#{id}");
                VALUES("password", "#{password}");
                VALUES("name", "#{name}");
            }}.toString();
        }
    }

}
```

Select문과 마찬가지로 SQL을 작성하듯이 작성하면 된다. 동일하게 if문을 사용할 수 있다.



### 마지막으로

Delete, Update또한 동일한 방식으로 사용하면 된다. 위쪽에서 Providing해주는 함수의 어노테이션을 변경해야한다는 점만 주의하면 된다.

또 위에서 SQL객체를 생성할 때 사용한 방법은 Java의 Double Brace이다.
