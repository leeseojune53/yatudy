# NestJs와 MongoDB로 위치정보 사용하기 

### 🎊 시작하기 전에...

NestJS와 MongoDB를 TypeORM을 이용해서 위치정보를 활용 방법을 적은 글입니다.

### 1️⃣ 스키마 정의

```typescript
export class Location {
  constructor(type: string, coordinates: number[]) {
    this.type = type;
    this.coordinates = coordinates;
  }
  type: string;
  coordinates: number[];
}
```

```typescript
@Entity()
export class Store {
  @ObjectIdColumn()
  id: number;

  @Column()
  title: string;

  @Column()
  category: string;

  @Column({ length: 512, unique: true })
  address: string;

  @Column({ type: 'point' })
  location: Location; 

  @Column()
  x: number;

  @Column()
  y: number;
}
```

코드가 상당히 길지만, 중요한 부분만 요약하면, 위치 정보를 저장하는 location은 object type으로 두어야하는데, mongoDB에서 위치 정보가 저장될 때 

```
{"type": "Point", "coordinates": [126.8990639, 33.4397954]}
```

와 같이 저장이 되므로 위 형태를 객체로 만들어서 엔티티에서 사용을 하면 된다.

### 2️⃣ 주변 위치 찾기

```typescript
this.storeRepository
      .find({
        where: {
          $and: [
            {
              location: {
                $geoWithin: {
                  $centerSphere: [
                    [Number(lon), Number(lat)],
                    Number(distance) / 6378.1,
                  ],
                },
              },
            },
          ],
        },
      })
      .catch(() => {
        throw new BadRequestException('Parameter Value Not Found');
      });
```

위 코드에서 storeRepository는 TypeORM의 EntityRepository를 이용해서 만들어진 Repository이다.

위치 기반 검색에서 `centerSphere`라는 방법으로 기준 위치 `[Number(lon), Number(lat)]`와 거리차이 `Number(distance) / 6378.1` (KM단위)로 값을 넣어주면 된다.

> 6378.1은 지구 둘레이다.