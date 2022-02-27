# NestJS에서 Strategy Pattern 적용하기

### 🎊 시작하기 전에...

이 글에서는 프로덕트를 리팩토링할 때 불가피하게 두 개의 DAO를 사용하는 상황을 중심으로 적었다 (TypeORM을 이용해서 plain-sql DAO, Model을 이용한 DAO를 사용하는 상황)

### 1️⃣ 추상 클래스 정의

우선, 여러 DAO의 부모가 될 추상 클래스를 정의한다.

```typescript
export abstract class IWeaponRepository {
  abstract findAll(): Promise<Weapon[]>;
}
```

### 2️⃣ 구현 클래스 정의

TypeORM의 Repository를 사용하는 경우 추상클래스를 **상속(extends)**이 아닌 **구현(implements)**를 사용해야 하는데, TypeORM Model을 이용한 Repository를 상속받아서 사용하기 때문에 **이중 상속이 되기 때문**이다.

```typescript
@EntityRepository(Weapon)
export class WeaponRepository
  extends Repository<Weapon>
  implements IWeaponRepository
{
  async findAll(): Promise<Weapon[]> {
    return await this.findAll();
  }
}

```

```typescript
@Injectable()
export class WeaponSqlRepository implements IWeaponRepository {
  constructor(@InjectConnection() private readonly connection: Connection) {}

  async findAll(): Promise<Weapon[]> {
    console.log(await this.connection.query(`SELECT * FROM tbl_weapon`));
    return await this.connection.query(`SELECT * FROM tbl_weapon`);
  }
}
```

### 3️⃣ 모듈 설정

NestJS 모듈에서 provides에 전략 패턴에서 사용 될 구현체를 지정해줘야 한다.

```typescript
@Module({
  ...
  providers: [
    CharacterService,
    { provide: IWeaponRepository, useClass: WeaponSqlRepository },
  ],
})
export class AppdataModule {}
```

위와 같이 **provide에는 추상 클래스**를 입력하고, **useClass에는 사용 될 구현체**를 적으면 된다.