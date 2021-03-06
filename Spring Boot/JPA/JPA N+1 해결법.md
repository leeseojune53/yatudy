# JPA N+1 ํด๊ฒฐ๋ฒ

### ๐ ๋ฌธ์  ์ํฉ

**์ฐ๊ด ๊ด๊ณ**์์ ๋ฐ์ํ๋ ์ด์๋ก ์ฐ๊ด ๊ด๊ณ๊ฐ ์ค์ ๋ ์ํฐํฐ๋ฅผ **์กฐํ**ํ  ๊ฒฝ์ฐ์ **์กฐํ๋ ๋ฐ์ดํฐ ๊ฐฏ์**๋งํผ ์ฐ๊ด๊ด๊ณ์ **์กฐํ ์ฟผ๋ฆฌ๊ฐ ์ถ๊ฐ๋ก ๋ฐ์**ํ์ฌ ๋ฐ์ดํฐ๋ฅผ ์ฝ์ด์ค๊ฒ๋.

### ๐ดโโ ๏ธ ์์ธ

์ฐ๊ด ๊ด๊ณ์์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ง๊ณ  ์ฌ ๋ join์ ์ฌ์ฉํ์ง ์๊ณ , ๊ฐ ๋ฐ์ดํฐ ๋ณ๋ก `where ~id = ?` ์ ๊ฐ์ด ์ฌ๋ฌ ๋ฒ ๋ฐ๋ณตํด์ ๊ฐ์ง๊ณ  ์ค๊ธฐ ๋๋ฌธ.

### โป ํด๊ฒฐ๋ฒ

### join fetch

```java
@Query("select a from Academy a join fetch a.subjects")
List<Academy> findAllJoinFetch();
```

์กฐํ ์ ๋ฐ๋ก ๊ฐ์ ธ์ค๊ณ  ์ถ์ Entity ํ๋๋ฅผ ์ง์  (join fetch a.subjects)์ฒ๋ผ ํ๋ ๊ฒ

### @EntityGraph

```java
@EntityGraph(attributePaths = "subjects")
```

@EntityGraph์ attributePaths์ ์ฟผ๋ฆฌ ์ํ ์ ๋ฐ๋ก ๊ฐ์ ธ์ฌ ํ๋๋ช์ ์ง์ ํ๋ฉด Lazy๊ฐ ์๋ Eager ์กฐํ, ์ฆ์ ์กฐํ๋ก ๊ฐ์ ธ์ค๊ฒ ๋๋ค.

### ๐ต ์ฐจ์ด์ 

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

JoinFetch๋ **Inner Join**, Entity Graph๋ **Outer Join**์ด๋ค.

ํด๊ฒฐ๋ฐฉ๋ฒ์ผ๋ก๋

1. @Entity Graph๋ฅผ ์ฌ์ฉํ  ๋ @OneToMany๊ฐ ๋ถ์ด์๋ ํ๋์ ํ์์ **Set**์ผ๋ก ์ ์ธํ๋ ๊ฒ์ด๋ค.

2. distinct๋ฅผ ์ฌ์ฉํ์ฌ ์ค๋ณต์ ์ ๊ฑฐํ๋ ๊ฒ์ด๋ค.

