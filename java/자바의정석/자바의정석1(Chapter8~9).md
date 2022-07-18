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

----

## Chapter 9. Java.lang 패키지와 유용한 클래스

1. Object 클래스

   1. equals(Object obj)

      1. 두 객체의 같고 다름을 참조변수의 값으로 판단한다. 그렇기 때문에 서로 다른 두 객체를 equals메서드로 비교하면 항상 false를 결과로 얻게 된다.
      2. 값을 비교하고 싶다면 equals 메서드를 오버라이딩하여 주소가 아닌 객체에 저장된 내용을 비교하도록 변경한다.

   2. hashCode()

      1. 해시함수는 찾고자하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드를 반환한다.
      2. 객체의 주소값을 이용해서 해시코드를 만들어 반환하기 때문에 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없다.
      3. String 클래스는 문자열의 내용이 같으면, 동일한 해시코드를 반환하도록 hashcode메서드가 오버라이딩되어 있기 때문에, 문자열의 내용이 같은 str1과 str2에 대해 항상 동일한 해시코드값을 얻는다. 반면에 System.indentityHaseCode(Object x)는 Object 클래스의 hashCode 메서드처럼 객체의 주소값으로 해시코드를 생성하기 때문에 모든 객체에 대해 항상 다른 해시코드값을 반환할 것을 보장한다.

   3. toString()

      1. ```java
         public String toString() {
         	return getClass().getName()+"@"+Integer.toHexString(hashCode());
         }
         ```

      2. 일반적으로 인스턴스나 클래스에 대한 정보 또는 인스턴스 변수들의 값을 문자열로 변환하여 반환하도록 오버라이딩 되는 것이 일반적이다.

   4. clone()

      1. 자신을 복제하여 새로운 인스턴스를 생성하는 일을 한다.
      2. Object 클래스에 정의된 clone()은 단순히 인스턴스 변수의 값만을 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다. 예를 들어 배열의 경우, 복제된 인스턴스도 같은 배열의 주소를 갖기 때문에 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미치게 된다. 이런 경우 clone 메서드를 오버라이딩 해서 새로운 배열을 생성하고 배열의 내용을 복사하도록 해야 한다.

   5. getClass()

      1. 자신이 속한 클래스의 Class객체를 반환하는 메서드이다.

2. String 클래스

- 변경 불가능한(immutable) 클래스

  1. 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스 변수에 문자형 배열(char[])로 저장된다.

  2. String 클래스는 앞에 final이 붙어 있으므로 다른 클래스의 조상이 될 수 없다.
     ```java
     public final class String implements java.io.Serializable, Comparable {
     	private char[] value;
     }
     ```

  3. 한번 생성된 String 인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다. 이처럼 String 값은 연산 시 마다 새로운 문자열을 가진 String 인스턴스가 생성되어 메모리 공간을 차지하게 되므로 가능한 한 결합횟수를 줄이는 것이 좋다.
     문자열의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 String 클래스 대신 StringBuffer 클래스를 사용하는 것이 가능하다.
     StringBuffer 인스턴스에 저장된 문자열은 변경 가능하다.

- 문자열을 만드는 두 가지 방법

  ```java
  // 1. 문자열 리터럴을 지정하는 방법 - str1,str2는 같은 주소를 가리킨다.
  String str1 = "abc";
  String str2 = "abc";
  
  // 2. String 클래스의 생성자를 사용해서 만드는 방법 - str3,str4는 다른 주소를 가리킨다.
  String str3 = new String("abc");
  String str4 = new String("abc");
  ```

  



























