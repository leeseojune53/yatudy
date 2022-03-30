# OrElse vs OrElseGet

### 😵 차이점

OrElse()는 Optional의 **값을 비교하기 전**에 OrElse() **내부의 값을 가져오고**,

OrElseGet()은 Optional의 **값을 비교한 후** OrElseGet() **내부의 값을 가져온다**.

큰 차이점으로 안느껴지지만, OrElse() 내부의 값을 가져오는데 **시간이 많이 걸린다면 불필요하게 시간**이 많이 걸리게된다.