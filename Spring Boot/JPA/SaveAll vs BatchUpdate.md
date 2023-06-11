# SaveAll vs BatchUpdate

## 개요

회사에서 사내관리 프로젝트를 개선하면서 평균적으로 1000건 이상 데이터가 한 번에 들어가는 Api가 존재했다. 이 때 JPA에서 제공해주는 saveAll로 코드를 작성하였을 때 너무 오랜 시간이 걸렸고 Async function으로 분리해서 오류가 생기면 slack notice를 주게 만들었었다.

하지만, Async로 처리하면 해당 함수에서 오류가 발생해도 이전에 생성된 부분은 Rollback되지 않기 때문에 Async function으로 분리하지 않고 사용할 방법을 찾다 BatchUpdate를 찾게되었다.

항상 새로운 데이터를 비교할 필요없이 INSERT하면 되었기에 batchUpdate를 사용하였고, 비즈니스 안정성과 성능 둘 다 잡게되어 글을 통해 공유한다.

### SaveAll vs BatchUpdate 성능

- CPU : M1 PRO
- RAM : 16GB
- Repo : https://github.com/leeseojune53/TIL/tree/master/BackEnd_Learn/Java/saveall-vs-batch

| 개수  | saveAll(초)          | batchUpdate(초)      |
| ----- | -------------------- | -------------------- |
| 10    | 0.213 (개당 0.0213)  | 0.361 (개당 0.0361)  |
| 100   | 1.422 (개당 0.0142)  | 0.728 (개당 0.0072)  |
| 1000  | 16.031 (개당 0.0160) | 4.244 (개당 0.0042)  |
| 10000 | 666 (개당 0.0666)    | 36.012 (개당 0.0036) |

아래 순서대로 10, 100, 1000, 10000개 입력이다.

![10row.png](https://github.com/leeseojune53/yatudy/blob/main/images/research/10row.png?raw=true)

![100row.png](https://github.com/leeseojune53/yatudy/blob/main/images/research/100row.png?raw=true)

![1000row.png](https://github.com/leeseojune53/yatudy/blob/main/images/research/1000row.png?raw=true)

![10000row.png](https://github.com/leeseojune53/yatudy/blob/main/images/research/10000row.png?raw=true)



### 주의점

Batch size, 환경에 따라 성능 차이가 달라질 수 있다.

### 결론

따라서, Dirty checking 등 JPA의 도움 필요없이, 순수하게 값을 넣거나(INSERT), 업데이트하는데 들어가는 요소의 개수가 많다면 BatchUpdate를 사용해보는 것도 좋은 방법이라고 생각한다.