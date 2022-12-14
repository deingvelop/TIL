# TDD(Test-Driven Development)란?

![](https://velog.velcdn.com/images/sw_smj/post/ec90a437-7f5c-4d70-876f-c221e2ce283c/image.png)


- AS-IS (설계 → 개발 → 테스트 → 설계 수정) 이었던 순서를
- TO-BE (설계 → 테스트 → 설계 수정 → 개발)로 바꾸는 것

<br>

## 테스트 코드


: 소프트웨어의 제품 or 서비스의 품질을 확인하거나 소프트웨어의 버그를 찾을 때 작성하는 코드. 즉, 제품이 예상하는 대로 동작 하는지 확인하는 코드

<br>


### 테스트 코드 작성 시 장점

- 빠르고 정확한 테스트 가능 (예상 동작 vs 실제 동작)

- 테스트 자동화 가능 (배포 절차시 테스트 코드가 수행되어 동작 검증)

- 결함을 사전에 발견할 수 있음

- 리팩토링한 후, 리팩토링한 소스가 기존의 소스와 동일한 동작을 하는지에 대한 걱정을 하지 않아도 됨. 일종의 보증이 생김

- 문서로서 작용할 수 있음 : 코드 작성자의 의도, 사용법, 주의사항 등이 드러남

<br>

### 테스트 코드 작성 시 단점

- 개발 시간이 오래 걸림

- 테스트 코드를 유지보수하는 비용이 별도로 들음

<br><br>

## 테스트의 종류와 특징


### 단위 테스트 (Unit Test)



: 프로그램을 작은 단위로 쪼개서 각 단위가 정확하게 동작하는지 검사하는 것

- 문제 발생시 문제가 발생한 부분을 빠르고 명확하게 확인할 수 있다.

- 일반적으로 클래스 또는 메서드 단위로 실행하며 단위가 작을수록 복잡성이 낮아진다.

- 자신이 작성한 코드에 대해 테스트 코드를 작성하며 구현된 코드의 내용을 알고 있어야 하는 화이트 박스 테스트이다.

- 단위 모듈에 대한 검사만 하기 때문에, 모듈 간에 상호 작용은 검증할 수 없다.

<br>

### 통합 테스트 (Integration Test)

- 최소 두 개 이상의 서브 시스템을 결합하여 테스트하는 것을 말한다.

- 모듈 간의 연결에서 발생하는 에러를 검증할 수 있다.

- ex : postman 등을 이용하여 서버와 DB서버가 정상적으로 통신하고 올바른 결과물을 출력하는지 확인하는 것

- 단위 테스트보다 더 많은 코드를 실행하기 때문에 에러가 어디서 발생했는지 파악하거나 수정하는 것이 비교적 어렵다.

<br>

### E2E 테스트 (End to End Test, = UI 테스트)

- 실제 사용자의 실행 환경과 거의 동일한 환경에서 테스트를 진행한다. (= 블랙박스 테스팅)

- 테스트 중 가장 까다롭고 비용이 많이 드는 테스트이다.

<br>

### Testing pyramid

![](https://velog.velcdn.com/images/sw_smj/post/2a2e6b4b-7840-4343-9297-81883f9f5353/image.png)

Google Test Automation Conference에서 제안된 Testing pyramid에 따르면, **전체 테스트의 비율**은
- **`E2E Test` 10%**
- **`Intergration Test` 20%**
- **`Unit Test` 70%**

를 유지하는 것이 이상적이라고 한다.
