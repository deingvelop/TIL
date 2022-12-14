# 객체 생성 패턴

객체를 생성하기 위해서는 다음과 같은 패턴들을 사용할 수 있다.

- 생성자 패턴
- 정적 메소드 패턴
- 수정자 패턴
- 빌더 패턴

<br>

# Builder Pattern

```java
User user = User.builder()
			.name("테스트")
            .age(19)
            .height(181)
            .iq(121).build();
```


### 장점
**1. 필요한 데이터만 설정할 수 있음**<br>
  - 테스트용 객체 생성에도 용이함
  - 불필요한 코드의 양을 줄일 수 있음

**2. 유연성을 확보할 수 있음**
- ex: 예를 들면, 클래스에 새로운 변수를 추가해야 할 때, 기존의 코드의 양이 방대하다면 클래스를 수정하는 것이 불리할 수 있다.
  ➡ 이때 빌더 패턴을 사용하면 기존의 코드에 영향을 주지 않을 수 있다.

**3. 가독성을 높일 수 있음**
- 생성자로 객체를 생성하는 경우, 매개변수가 많아질수록 코드 리딩이 급격히 떨어진다.
  ➡ 이때, 빌더 패턴을 적용하면 직관적으로 어떤 데이터에 어떤 값들이 설정되는지 쉽게 파악 가능하다.

**4. 변경 가능성을 최소화할 수 있음**
- 많은 개발자들이 수정자(`Setter`)를 사용한다. 그러나 `Setter`는 불필요하게 변경 가능성을 열어둔다는 단점이 있다. → 유지보수시 값이 할당된 지점을 찾기 어려움
  ➡ 클래스 변수는 변경 가능성을 최소화하는 것이 좋음
  ➡ 변수를 `final`로 선언 → 불변성 확보


<br>

## 주의사항

객체를 생성하는 대부분의 경우에는 빌더 패턴을 적용하는 것이 좋다.

단, 다음의 2가지 상황에서는 빌더를 구현할 필요가 없다.

- **객체의 생성을 라이브러리로 위임하는 경우**
    - ex) Entity 객체, Domain 객체로부터 DTO를 생성하는 경우 : 빌더를 통한 생성이 번거로움→ MapStruct, Model Mapper 등의 라이브러리를 통해 생성을 위임할 수 있음 ➡ 훨씬 편리!

- **변수의 개수가 2개 이하이며, 변경 가능성이 없는 경우**
    - 이런 경우는 정적 팩토리 메소드를 사용하는 것이 더 좋을 수도 있다.


빌더의 남용은 오히려 코드를 비대하게 만들 수 있으므로, ***변수의 개수***와 ***변경 가능성*** 등을 중점적으로 보고 빌더 패턴을 적용할지 판단하면 되겠다.