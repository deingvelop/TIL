# Lambda 함수



: 프로그래밍 언어에서 사용되는 개념으로, 익명함수(Anonymous functions)를 지칭하는 용어

> 📌 **용어 유래**
> 현재 사용되고 있는 **람다**의 근간은 수학과 기초 컴퓨터과학 분야에서의 람다 대수이다.
>
> **람다 대수**란, 간단히 말하자면 수학에서 사용하는 함수를 보다 단순하게 표현하는 방법이다.

<br>

### 특징

- 익명함수 : Lambda 대수는 이름을 가질 필요가 없다.

- 커링(Curring) : 두 개 이상의 입력이 있는 함수는 최종적으로 1개의 입력만 받는 람다 대수로 단순화될 수 있다.

<br>

### 장점

- **코드의 간결성** : 불필요한 반복문의 삭제가 가능하며, 복잡한 식을 단순하게 표현할 수 있다.

- **지연연산 수행** : 람다는 지연연산을 수행함으로써 불필요한 연산을 최소화할 수 있다.

  > ✍🏻 **지연연산이란?**
  >
  > 코드의 실행이 불필요할 경우 실행을 연기한다는 것이다.
  > 즉, 주어진 조건에 따라 실제로 계산될 필요가 없는 함수는 그 함숫값이 필요한 시점까지 계산하지 않고 지연하여 불필요한 계산을 피하는 방법이다.

- **병렬처리 가능** : 멀티쓰레드를 활용하여 병렬처리를 할 수 있다.

<br>

### 단점

- 람다식의 호출이 까다롭다.

- 람다 stream 사용시 단순 for문 또는 while문을 사용하면 성능이 떨어진다.

- 불필요하게 너무 사용하게 되면 오히려 가독성을 떨어뜨린다.

<br>

## Lambda의 표현식

- `매개변수 -> 함수몸체`로 이용하여 사용할 수 있다.

- 함수몸체가 단일 실행문이면 괄호`{}`를 생략할 수 있다.

- 함수 몸체가 return문으로만 구성되어있는 경우, 괄호`{}`를 생략할 수 없다.
    - case 1

      ```java
      (int x) -> x+1
       (x) -> x+1
       x -> x+1
       (int x) -> {return x+1;}
       x -> {return x+1;}
      ```

    - case 2
      ```java
      (int x, int y) -> x+y
       (x, y) -> x+y
       (x, y) -> {return x+y;}
       ```

    - case 3
      ```java
      (String lam) -> lam.length()
       lam -> lam.length()
       (Thread lamT) -> {lamT.start();}
       lamT -> {lamT.start();}
      ```
