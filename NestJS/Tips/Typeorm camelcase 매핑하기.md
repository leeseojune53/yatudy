# Typeorm camelcase 매핑하기

### 🎊 시작하기 전에...

이 글은 Typeorm을 사용할 때 Entity 객체에 column naming을 snake case로 해야해서 **네이밍 규칙이 ts와 맞지않는 불편함**을 라이브러리를 활용해서 해결하는 방법을 적었다.

### 1️⃣ 첫 번째로

`npm install typeorm-naming-strategies`를 실행해서 라이브러리를 설치해준다.

### 2️⃣ 두 번째로

TypeormModule 설정 시 **namingStrategy에 SnakeNamingStrategy**를 넣어주면 된다.

```typescript
type: 'mysql',
host: process.env.DEVELOPMENT_DATABASE_HOST,
port: +process.env.DEVELOPMENT_DATABASE_PORT,
username: process.env.DEVELOPMENT_DATABASE_USER,
password: process.env.DEVELOPMENT_DATABASE_PASSWORD,
database: process.env.DEVELOPMENT_DATABASE_NAME,
synchronize: false,
logging: true,
entities: ['./dist/**/*.entity.js'],
namingStrategy: new SnakeNamingStrategy()
```

### 3️⃣ 세 번째로

Entity 객체에 snake case로 되어있는 필드를 **camel case로 변경**한다.