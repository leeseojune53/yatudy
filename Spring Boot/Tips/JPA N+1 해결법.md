# JPA N+1 해결법

### 🐛 문제상황

연관 관계에서 발생하는 이슈로 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게됌.

### 🏴‍☠️ 원인

연관 관계에서 데이터를 가지고 올 때 join을 사용하지 않고, 각 데이터 별로 `where ~id = ?` 와 같이 여러 번 반복해서 가지고 오기 때문.

### ♻ 해결법

### join fetch

```java
@Query("select a from Academy a join fetch a.subjects")
List<Academy> findAllJoinFetch();
```

조회 시 바로 가져오고 싶은 Entity 필드를 지정 (join fetch a.subjects)처럼 하는 것

### @EntityGraph

```java
@EntityGraph(attributePaths = "subjects")
```

@EntityGraph의 attributePaths에 쿼리 수행 시 바로 가져올 필드명을 지정하면 Lazy가 아닌 Eager 조회, 즉시 조회로 가져오게 된다.

### 😵 차이점

**join fetch**

```sql
SELECT academy0_.id          AS id1_0_0_, 
       subjects1_.id         AS id1_1_1_, 
       academy0_.name        AS name2_0_0_, 
       subjects1_.academy_id AS academy_3_1_1_, 
       subjects1_.name       AS name2_1_1_, 
       subjects1_.teacher_id AS teacher_4_1_1_, 
       subjects1_.academy_id AS academy_3_1_0__, 
       subjects1_.id         AS id1_1_0__ 
FROM   academy academy0_ 
       INNER JOIN subject subjects1_ 
               ON academy0_.id = subjects1_.academy_id 
```

**@Entity Graph**

```sql 
SELECT academy0_.id          AS id1_0_0_, 
       subjects1_.id         AS id1_1_1_, 
       academy0_.name        AS name2_0_0_, 
       subjects1_.academy_id AS academy_3_1_1_, 
       subjects1_.name       AS name2_1_1_, 
       subjects1_.teacher_id AS teacher_4_1_1_, 
       subjects1_.academy_id AS academy_3_1_0__, 
       subjects1_.id         AS id1_1_0__ 
FROM   academy academy0_ 
       LEFT OUTER JOIN subject subjects1_ 
                    ON academy0_.id = subjects1_.academy_id 
```

JoinFetch는 **Inner Join**, Entity Graph는 **Outer Join**이다.

해결방법으로는

1. @Entity Graph를 사용할 때 @OneToMany가 붙어있는 필드의 타입을 **Set**으로 선언하는 것이다.

2. distinct를 사용하여 중복을 제거하는 것이다.

