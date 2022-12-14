# 저장하는 값에 따른 구분
<br>

## 1. 기본형 변수


: 실제 값을 저장함
```java
int i = 0;
```
- stack영역에 선언한 변수만큼의 공간이 생성됨 → 그 공간에 값이 들어감

<br>

## 2. 참조형 변수


: 주소값을 저장함
```java
String s = new String("Java");
```
- 기본형 변수처럼 stack영역에 참조형변수명 s라는 이름을 가진 공간이 생성된다. 단, 이때 변수의 크기는 4byte의 고정된 크기만 생성된다.

- 그리고 `new`라는 명령어가 Heap 영역에 새로운 저장공간을 생성한다.
- 공간의 크기는 변수의 주소가 가리키는 실제 값(Java)의 크기만큼 생성된다.
- 그리고 이 공간은 메모리 주소값을 할당받는다.

<br><br><br>

# 변수의 선언 위치에 따라 구분
<br>

## 클래스 영역


: 접근 제어자에 따라 클래스를 외부에서도 사용할 수 있음
> 💡 **접근 제어자(Access Modifier)란?**
> - Java에서 정보 은닉을 위해 사용하는 것
> - 클래스 외부에서의 직접적인 접근을 허용하지 않는 멤버를 설정하녀 정보 은닉을 구체화함
>
>
> - 접근 제어자 종류
>   - private
>   - public
>   - default
>   - protected

<br>

## 1. 클래스변수 (class variable)
&nbsp;&nbsp;&nbsp; = static 변수
- **클래스 영역**에서 타입 앞에 static이 붙는 변수
- 객체를 공유하는 변수
- 여러 객체에서 공통으로 사용하고 싶을 때 정의


- **접근 방법**
    - 클래스명
    - 클래스변수명

  > 👉🏻 **인스턴스 변수 vs 클래스 변수**
  > - **인스턴스 변수** : 객체(인스턴스)를 생성한 후 참조변수를 통해서만 접근 가능
  > - **클래스 변수** : 객체를 생성하지 않아도 클래스명으로 바로 접근이 가능

<br>

## 2. 인스턴스 변수 (instance variable)
- **클래스 영역** 중 static이 아닌 변수
- 개별적인 저장 공간으로 객체/인스턴스마다 다른 값 저장 가능
- 즉, 인스턴스를 여러 개 생성했다면 각각의 참조 변수명으로 접근 가능. 각각에 값 저장 가능

  > 👉🏻 **인스턴스 변수 vs 클래스 변수**
  > - **인스턴스 변수** : 객체를 공유하지 않음
  > - **클래스 변수** : 객체를 공유함

- **접근 방법** : 객체 생성 후 참조변수명으로 접근

  > 💡 **Tip**<br>
  > Java에서 객체/인스턴스를 생성만 하고 참조변수가 없는 경우, 가비지컬렉터에 의해 자동으로 제거됨


<br>

## 메서드 영역


: 접근제어자를 사용하지 않음


<br>

## 3. 지역 변수 (local variable)
- **메서드 내**에서 선언되고, 메서드 수행이 끝나면 소멸되는 변수
- 초기값을 지정한 후 사용할 수 있음
  ```java
  // example
  int i = 0;
  ```
  > 💡 **주의**<br>
  > 조건문, 반복문 밖에서도 변수를 사용하고 싶을 경우에는 블록 밖에서 선언 필요
  > (지역변수로 하면 소멸됨)

<br>

## 4. 매개변수 (parameter)

- **메서드 호출**시 '전달하는 값'을 가지고 있는 변수
- 즉 함수를 정의할 때, 전달받은 인수를 함수 내부로 전달하기 위해 사용하는 변수
- 지역변수와 마찬가지로 수행이 끝날때까지만 유효함
