# Java의 정석 1 (Chapter 8~9)

### 2021/07/12~2021/07/14

----

## Chapter 8. 예외처리

#### 프로그램 오류

1. 에러: 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
   1. 컴파일 에러: 컴파일 시에 발생하는 에러
   2. 런타임 에러: 실행 시에 발생하는 에러
   3. 논리적 에러: 실행은 되지만, 의도와 다르게 동작하는 것
2. 예외: 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

![aaa](/Users/youngjin/Downloads/aaa.png)

- Exception 클래스들: 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
- RuntimeException 클래스들: 프로그래머의 실수로 발생하는 예외

#### try-catch문 흐름

- Case1: try 블럭 내에서 예외가 발생한 경우
  1. 발생한 예외와 일치하는 catch 블럭이 있는지 확인한다.
  2. 일하는 catch 블럭을 찾게 되면, 그 catch 블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다.
     만일 일치하는 catch 블럭을 찾지 못하면, 예외는 처리되지 못한다.
- Case2: try 블럭 내에서 예외가 발생하지 않은 경우
  1. catch 블럭을 거치지 않고 전체 try-catch 문을 빠져나가서 수행을 계속한다.

- try-catch 문의 마지막에 Exception 클래스 타입의 참조변수를 선언한 catch 블럭을 사용하면, 어떤 종류의 예외가 발생하더라도 처리 가능하다.

#### printStackTrace(), getMessage()

- printStackTrace(): 예외발생 당시의 호출스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
- getMessage(): 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

#### 예외 발생시키기

```java
throw new Exception("고의로 예외 발생 시킴"); // checked 예외 - 컴파일 오류
throw new RuntimeException(); // unchecked 예외 - 컴파일 정상 작동
```

#### 메서드에 예외 선언하기

```java
vod method() throws Exception1, Exception2, ... ExceptionN {
  // 메서드의 내용
}
```

#### finally 블럭

Case1: 예외가 발생한 경우

- try - catch - finally

Case2: 예외가 발생하지 않은 경우

- try - finally





