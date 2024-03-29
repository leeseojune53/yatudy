# TinyInt vs Bit

### 🎊 시작하기 전에...

이 글은 어떤 것이 옳다를 이야기하는 것이 아니라 각 타입이 어떤 장단점을 가지고있는지를 다룬다.

또, MySQL을 기준으로 작성되었으며 버전에 따라 조금씩 상이할 수 있다.

### 1️⃣ 첫 번째로

`BIT(1)`과 `TINYINT`가 차지하는 크기는 아래와 같다

- BIT(1) : 1 Bit
- TINYINT(1) : 1 Byte

하지만, 여기에 함정이 있는데 `BIT(1)`도 점유하는 공간의 크기는 1 Byte라는 것이다. (물론, 여러 BIT컬럼이 있다면 동일한 Byte 내 채워진다.)

### 2️⃣ 두 번째로

MySQL에서 BOOL type은 TINYINT(1)을 지칭하는 말과 같다.

또, TINYINT는 BIT와 대조적으로 8-bit Bitmask를 안정적 사용할 수 있다. (BIT는 다른 BIT컬럼이 온다면 영향을 받을 수 있음.)