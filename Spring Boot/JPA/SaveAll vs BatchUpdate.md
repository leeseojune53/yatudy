# SaveAll vs BatchUpdate





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



따라서, Dirty checking 등 JPA의 도움 필요없이, 순수하게 값을 넣거나(INSERT), 업데이트하는데 들어가는 요소의 개수가 많다면 BatchUpdate를 사용해보는 것도 좋은 방법이라고 생각한다.