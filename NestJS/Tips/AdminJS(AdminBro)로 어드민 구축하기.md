

# AdminJS(AdminBro)로 어드민 구축하기

### 🎊 시작하기 전에...

AdminJS는 DB를 웹으로 제어할 수 있는 라이브러리이다. NodeJS 기반 어플리케이션에서 사용 가능하다.

이 글에서는 NestJS에서 AdminJS를 사용하는 방법을 소개한다.

### 1️⃣ 첫 번째로

`@adminjs/express @adminjs/nestjs @adminjs/typeorm adminjs express typeorm mysql2 express-formidable express-session`

위 라이브러리들을 추가해준다.

### 2️⃣ 두 번째로

Entity class는 **BaseEntity를 꼭 상속** 해줘야한다.

``` typescript
@Entity()
export class User extends BaseEntity {
  
}
```

### 3️⃣ 세 번째로

App Module에서 TypeORM과 AdminJS 설정을 해준다.

``` typescript
imports: [
	TypeOrmModule.forRoot({
      type: 'mysql',
      host: '주소',
      port: 포트번호,
      username: '아이디',
      password: '비밀번호',
      database: 'DB 명',
      entities: [Entity class],
 }),
 AdminModule.createAdmin({
      adminJsOptions: {
        rootPath: '/admin',
        resources: [
          Notification,
          {
            resource: Guide,
            options: { editProperties: ['temp', 'hide', 'blocked'] },
          },
        ],
      },
      auth: {
        authenticate: async (email, password) => {
          if (
            email === "아이디(이메일)" &&
            password === "비밀번호"
          ) {
            return {
              email: "아이디(이메일)",
              password: "비밀번호",
            };
          }
          return null;
        },
        cookieName: 'test',
        cookiePassword: process.env.DEVELOPMENT_ADMIN_PASSWORD,
      },
    }),
]
```

위 코드에서 `resources`에서 그냥 Entity class를 넣어도 되지만, 다른 제약조건을 걸려면 Guide entity와 같이 따로 옵션을 기재해주면 된다.

auth 부분은 인증 부분인데, 들어온 아이디 비밀번호를 받아서 올바른 어드민인지 확인하는 절차이다