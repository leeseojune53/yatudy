# NestJs로 Jwt 인증하기

### 🎊 시작하기 전에..

NestJs의 **Guard**에 대해서 알아야한다.

### 1️⃣ 가장 먼저

인증할 때 필요한 라이브러리들을 설치해야 한다.

```sh
npm install --save @nestjs/passport passport passport-local
npm install --save @nestjs/jwt passport-jwt
```

### 2️⃣ 두 번째로

Jwt guard를 작성한다.

```tsx
//jwt-auth.guard.ts

import {Injectable} from "@nestjs/common";
import {AuthGuard} from "@nestjs/passport";


@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
}
```

위에서 AuthGuard에 들어가있는 값은 strategy의 이름이다.

### 3️⃣ 세 번째로

Jwt Strategy를 작성한다.

```tsx
//jwt.strategy.ts

import {JWT_SECRET} from "../../auth/constants";
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';
import { Injectable } from '@nestjs/common';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy, 'jwt') {
    constructor() {
        super({
            jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
            ignoreExpiration: false,
            secretOrKey: JWT_키,
        });
    }

    public async validate(payload: any) {
        return { userId: payload.sub, username: payload.username };
    }

}
```

super에 들어가있는 Property값을 보자

- jwtFromRequest : JWT를 가져오는 **방법**, 위에서는 bearer를 떼고 가져오는 것으로 설정했다.
- ignoreExpiration : 만료된 토큰을 허용하는 여부.
- secretOrKey : JWT를 발급할 때 사용한 **서명(secretKey)**.

### 4️⃣ 네 번째로

모듈에서 위에서 작성한 부분의 의존성을 등록해야한다.

```ts
//auth.module.ts

@Module({
    imports: [
        PassportModule,
    ],
    providers: [AuthService, JwtStrategy],
    exports: [AuthService],
    controllers: [AuthController]
})
export class AuthModule {
}
```

위는 예시이고, 실제로는 더욱 많은 의존성이 존재한다.