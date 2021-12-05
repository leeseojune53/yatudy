# Querydsl 기본 세팅

### 1️⃣ build.gradle에서 세팅

```java
buildscript {
	ext {
		queryDslVersion = "4.4.0"
	}
}
```

```java
implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
annotationProcessor(
		"javax.persistence:javax.persistence-api",
		"javax.annotation:javax.annotation-api",
		"com.querydsl:querydsl-apt:${queryDslVersion}:jpa")
```

```java
sourceSets {
	main {
		java {
			srcDirs = ["$projectDir/src/main/java", "$projectDir/build/generated"]
		}
	}
}
```

### 2️⃣ JpaFactory 설정

```java
@Configuration
public class QuerydslConfig {

	@PersistenceContext
	private EntityManager entityManager;

	@Bean
	public JPAQueryFactory jpaQueryFactory() {
		return new JPAQueryFactory(entityManager);
	}

}
```

