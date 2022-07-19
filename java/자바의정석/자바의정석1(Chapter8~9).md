# Java의 정석 1 (Chapter 8~9)

### 2021/07/12~2021/07/20

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

  

| 메서드                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| String(String s)                                             | 주어진 문자열(s)을 갖는 String 인스턴스를 생성한다.          |
| String(char[] value)                                         | 주어진 문자열(value)을 갖는 String 인스턴스를 생성한다.      |
| String(StringBuffer buf)                                     | StringBuffer 인스턴스가 갖고 있는 문자열과 같은 내용의 String 인스턴스를 생성한다. |
| char charAt(int index)                                       | 지정된 위치(index)에 있는 문자를 알려준다.                   |
| int compareTo(String str)                                    | 문자열(str)과 사전순서로 비교한다. 같으면 0을, 사전순으로 이전이면 음수를, 이후면 양수를 반환한다. |
| String concat(String str)                                    | 문자열(str)을 뒤에 덧붙인다.                                 |
| boolean contains(CharSequence s)                             | 지정된 문자열(s)이 포함되었는지를 검사한다.                  |
| boolean endsWith(String suffix)                              | 지정된 문자열(suffix)로 끝나는지 검사한다.                   |
| boolean equals(Object obj)                                   | 매개변수로 받은 문자열(obj)과 String 인스턴스의 문자열을 비교한다. obj가 String이 아니거나 문자열이 다르면 false를 반환한다. |
| boolean equalsIgnoreCase(String str)                         | 문자열과 String 인스턴스의 문자열을 대소문자 구분없이 비교한다. |
| int indexOf(int ch)                                          | 주어진 문자(ch)가 문자열에 존재하는지 확인하여 위치(index)를 알려준다. 못 찾으면 -1을 반환한다. |
| Int indexOf(int ch, int pos)                                 | 주어진 문자(ch)가 문자열에 존재하는지 지정된 위치(pos)부터 확인하여 그 위치(index)를 알려준다. 못 찾으면 -1을 반환한다. |
| int indexOf(String str)                                      | 주어진 문자열이 존재하는지 확인하여 그 위치(index)를 알려준다. 없으면 -1을 반환한다. |
| String intern()                                              | 문자열을 상수풀(constant pool)에 등록한다. 이미 상수풀에 같은 내용의 문자열이 있을 경우 그 문자열의 주소값을 반환한다. |
| int lastIndexOf(int ch)                                      | 지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 위치(index)를 알려준다. 못 찾으면 -1을 반환한다. |
| int lastIndexOf(String str)                                  | 지정된 문자열을 인스턴스의 문자열 끝에서부터 찾아서 위치(index)를 알려준다. 못 찾으면 -1을 반환한다. |
| int length()                                                 | 문자열의 길이를 알려준다.                                    |
| String replace(char old, char nw)                            | 문자열 중의 문자(old)을 새로운 문자(nw)로 바꾼 문자열을 반환한다. |
| String replace(CharSequence old, CharSequence nw)            | 문자열 중의 문자열(old)을 새로운 문자열(nw)로 모두 바꾼 문자열을 반환한다. |
| String replaceAll(String regex, String replacement)          | 문자열 중에서 지정된 문자열(regex)과 일치 하는 것을 새로운 문자열(replacement)로 모두 변환한다. |
| String replaceFirst(String regex, String replacement)        | 문자열 중에서 지정된 문자열(regex)과 일치 하는 것 중, 첫 번째 것만 새로운 문자열(replacement)로 변환한다. |
| String[] split(String regex)                                 | 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환한다. |
| String[] split(String regex, int limit)                      | 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담에 반환한다. 단, 문자열 전체를 지정된 수(limit)로 자른다. |
| boolean startsWith(String prefix)                            | 주어진 문자열(prefix)로 시작하는지 검사한다.                 |
| String substring(int begin)<br />String substring(int begin, int end) | 주어진 시작위치(begin)부터 끝 위치(end) 범위에 포함된 문자열을 얻는다. 이 때, 시작위치의 문자는 범위에 포함되지만, 끝 위치의 문자는 포함되지 않는다. |
| String toLowerCase()                                         | String 인스턴스에 저장되어 있는 모든 문자열을 소문자로 변환하여 반환한다. |
| String toString()                                            | String 인스턴스에 저장되어 있는 문자열을 반환한다.           |
| String toUpperCase()                                         | String 인스턴스에 저장되어 있는 모든 문자열을 대문자로 변환하여 반환한다. |
| String trim()                                                | 문자열의 왼쪽 끝과 오른쪽 끝에 있는 공백을 없앤 결과를 반환한다. 이 때 문자열 중간에 있는 공백은 제거되지 않는다. |
| static String valueOf(boolean b)<br />static String valueOf(char c)<br />static String valueOf(int i)<br />static String valueOf(long l)<br />static String valueOf(float f)<br />static String valueOf(double d)<br />static String valueOf(Object o) | 지정된 값을 문자열로 변환하여 반환한다.<br />참조 변수의 경우, toString()을 호출한 결과를 반환한다. |

replace, replaceAll 차이점

- replace는 문자열만 변환가능한데 비해서 replaceAll은 인자값에 정규식이 들어간다. 따라서 정규식을 이용하여 불특정 문자열을 변환할 수 있는 이점이 있다.

join()과 StringJoiner

```java
class Main {
  public static void main(String[] args){
    String animals = "dog,cat,bear";
    String[] arr = animals.split(",");
    
    System.out.println(String.join("-", arr)); // dog-cat-bear
    
    StringJoiner sj = new StringJoiner("/","[","]");
    for(String s : arr){
      sj.add(s)
    }
    System.out.println(sj.toSting()); // [dog/cat/bear]
  }
}
```

String.format() - printf()와 사용법이 같다.

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5);
System.out.println(str); // 3 더하기 5는 8입니다.
```



| 기본형 -> 문자열                                             | 문자열 -> 기본형                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| String String.valueOf(boolean b)<br />String String.valueOf(char c)<br />String String.valueOf(int i)<br />String String.valueOf(long l)<br />String String.valueOf(float f)<br />String String.valueOf(double d) | boolean Boolean.parseBoolean(String s)<br />byte Byte.parseByte(String s)<br />short Short.parseShort(String s)<br />int Integer.parseInf(String s)<br />long Long.parseLont(String s)<br />float Float.parseFloat(String s)<br />double Double.parseDouble(String s) |

- 예전에는 parseInt()와 같은 메서드를 많이 썼는데, 메서드의 이름을 통일하기 위해 valueOf()가 나중에 추가되었다.
  valueOf(String s)는 메서드 내부에서 그저 parseInf(String s)를 호출할 뿐이므로, 두 메서드는 반환 타입만 다르지 같은 메서드다.
  valueOf는 Integer를 반환하며, 오토박싱에 의해 Integer가 int로 자동 변환된다.



3. StringBuffer 클래스와 StringBuilder 클래스

   1. StringBuffer 
      - String 클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 StringBuffer 클래스는 변경이 가능하다.
        내부적으로 문자열 편집을 위한 버퍼를 가지고 있으며, StringBuffer 인스턴스를 생성할 때 그 크기를 지정할 수 있다.
        이 때, 편집할 문자열의 길이를 고려하여 버퍼의 길이를 충분히 잡아주는 것이 좋다. 편집 중인 문자열이 버퍼의 길이를 넘어서게 되면 버퍼의 길이를 늘려주는 작업이 추가로 수행되어야하기 때문에 작업효율이 떨어진다.
        버퍼의 크기를 지정해주지 않으면 16개의 문자를 저장할 수 있는 크기의 버퍼를 생성한다.

   | 메서드                                                       | 설명                                                         |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | StringBuffer()                                               | 16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다. |
   | StringBuffer(int length)                                     | 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다. |
   | StringBuffer(String s)                                       | 지정된 문자열 값(str)을 갖는 StringBuffer 인스턴스를 생성한다. |
   | StringBuffer append(boolean b)<br />StringBuffer append(char c)<br />StringBuffer append(char[] str)<br />StringBuffer append(double d)<br />StringBuffer append(float f)<br />StringBuffer append(int i)<br />StringBuffer append(long l)<br />StringBuffer append(Object obj)<br />StringBuffer append(String str) | 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer 인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다. |
   | int capacity()                                               | StringBuffer 인스턴스의 버퍼크기를 알려준다. length()는 버페에 담긴 문자열의 길이를 알려준다. |
   | char charAt(int index)                                       | 지정된 위치(index)에 있는 문자를 반환한다.                   |
   | StringBuffer delete(int start, int end)                      | 시작위치부터 끝 위치 사이에 있는 문자를 제거한다. 단, 끝 위치의 문자는 제외. |
   | StringBuffer deleteCharAt(int index)                         | 지정된 위치(index)의 문자를 제거한다.                        |
   | StringBuffer insert(int pos, boolean b)<br />StringBuffer insert(int pos, char c)<br />StringBuffer insert(int pos, char[] str)<br />StringBuffer insert(int pos, double d)<br />StringBuffer insert(int pos, float f)<br />StringBuffer insert(int pos, int i)<br />StringBuffer insert(int pos, long l)<br />StringBuffer insert(int pos, Object obj)<br />StringBuffer insert(int pos, String str) | 두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치(pos)에 추가한다. pos는 0부터 시작. |
   | int length()                                                 | StringBuffer 인스턴스에 저장되어 있는 문자열의 길이를 반환한다. |
   | StringBuffer replace(int start, int end, String str)         | 지정된 범위의 문자들을 주어진 문자열로 바꾼다. end 위치의 문자는 범위에 포함되지 않음. |
   | StringBuffer reverse()                                       | StringBuffer 인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열한다. |
   | void setCharAt(int index, char ch)                           | 지정된 위치의 문자를 주어진 문자(ch)로 바꾼다.               |
   | void setLength(int newLength)                                | 지정된 길이로 문자열의 길이를 변경한다. 길이를 늘리는 경우에 나머지 빈 공간을 널문자 '\u0000'로 채운다. |
   | String toString()                                            | StringBuffer 인스턴스의 문자열을 String으로 반환             |
   | String substring(int start)<br />String substring(int start, int end) | 지정된 범위 내의 문자열을 String으로 뽑아서 반환한다. <br />시작위치만 지정하면 시작위치부터 문자열 끝까지 뽑아서 반환한다. |

   

   2. StringBuilder
      - StringBuffer는 멀티쓰레드에 안전(thread safe)하도록 동기화되어 있다. 이 동기화는 StringBuffer의 성능을 떨어뜨리게 된다.
        그래서 StringBuffer에서 쓰레드의 동기화만 뺀 StringBuilder가 새로 추가되었다.
        StringBuffer도 충분히 성능이 좋기 때문에 성능향상이 반드시 필요한 경우를 제외하고는 기존에 작성한 코드에서 굳이 바꿀 필요는 없다.

4. Math 클래스

| 메서드                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| static double abs(double a)<br />static float abs(float f)<br />static int abs(int f)<br />static long abs(long f) | 주어진 값의 절대값을 반환한다.                               |
| static double ceil(double a)                                 | 주어진 값을 올림하여 반환한다.                               |
| static double floor(double a)                                | 주어진 값을 버림하여 반환한다.                               |
| static double max(double a, double b)<br />static float max(float a, float b)<br />static int max(int a, int b)<br />static long max(long a, long b) | 주어진 두 값을 비교하여 큰 쪽을 반환한다.                    |
| static double min(double a, double b)<br />static float min(float a, float b)<br />static int min(int a, int b)<br />static long min(long a, long b) | 주어진 두 값을 비교하여 작은 쪽을 반환한다.                  |
| static double random()                                       | 0.0~1.0 범위의 임의의 double 값을 반환한다.<br />(1.0)은 범위에 포함되지 않는다. |
| static double rint(double a)                                 | 주어진 double값과 가장 가까운 정수값을 double형으로 반환한다. |
| static long round(double a)<br />static long round(float a)  | 소수점 첫째자리에서 반올림한 정수값(long)을 반환한다. <br />매개변수의 값이 음수인 경우, round()와 rint()의 결과가 다르다는 것에 주의하자. |



5. 래퍼(wrapper) 클래스

![Java Wrapper 클래스 - Junhyunny's Devlogs](https://junhyunny.github.io/images/java-wrapper-class-1.JPG)

- 8개의 기본형 중 char, int형을 제외한 나머지는 자료형 이름의 첫 글자를 대문자로 한 것이 각 래퍼 클래스의 이름이다.

| 기본형  | 래퍼 클래스 | 생성자                                                       |
| ------- | ----------- | ------------------------------------------------------------ |
| boolean | Boolean     | Boolean (boolean value)<br />Boolean (String s)              |
| char    | Character   | Character (char value)                                       |
| byte    | Byte        | Byte (byte value)<br />Byte (String s)                       |
| short   | Short       | Short (short value)<br />Short (String s)                    |
| int     | Integer     | Integer (int value)<br />Integer (String s)                  |
| long    | Long        | Long (long value)<br />Long (String s)                       |
| float   | Float       | Float (double value)<br />Float (float value)<br />Float (String s) |
| double  | Double      | Double (double value)<br />Double (String s)                 |



| 문자열 -> 기본형                                             | 문자열 -> 래퍼 클래스                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| byte b = Byte.parseByte("100")<br />short s = Short.parseShort("100")<br />int i = Integer.parseInt("100")<br />long l = Long.parseLong("100")<br />float f = Float.parseFloat("3.14")<br />double d = Double.parseDouble("3.14") | Byte b = Byte.valueOf("100")<br />Short s = Short.valueOf("100")<br />Integer i = Integer.valueOf("100")<br />Long l = Long.valueOf("100")<br />Float f = Float.valueOf("3.14")<br />Double d = Double.valueOf("3.14") |

- JDK1.5부터 도입된 오토박싱 기능 때문에 반환값이 기본형일 때와 래퍼클래스일 때의 차이가 없어졌다.
  그래서 구별없이 valueOf()를 쓰는 것도 괜찮은 방법이다. 단, 성능은 valueOf()가 조금 더 느리다.

- 문자열의 진법 변환
  ```java
  static int parseInt(String s, int radix) // 문자열 s를 radix 진법으로 인식
  static Integer valueOf(String s, int radix)
  ```

6. 오토박싱 & 언박싱
   - JDK 1.5 이전에는 기본형과 참조형 간의 연산이 불가능했기 때문에, 래퍼 클래스로 기본형을 객체로 만들어서 연산해야 했다.
     하지만 이제는 기본형과 참조형 간의 덧셈이 가능하다. 
     자바 언어의 규칙이 바뀐 것은 아니고, 컴파일러가 자동으로 변환하는 코드를 넣어주기 때문이다.
   - 오토박싱: 기본형 -> 래퍼 클래스
   - 언박싱: 래퍼 클래스 -> 기본형

----

정규식 - java.util.regex 패키지

```java
import java.util.regex.*;

class RegularEx1 {
  public static void main(String[] args){
    String data = {"bat", "baby", "bonus", "cA", "ca", "co", "c.", "c0", "car", "combat", "count", "date", "disc"};
    Pattern p = Pattern.compile("c[a-z]*");
    
    for(int i=0; i<data.length; i++){
      Matcher m = p.matcher(data[i]);
      if(m.matches())
        System.out.println(data[i] + ",");
    }
  }
}
// ca,co,car,combat,count
```

1. 정규식을 매개변수로 Pattern 클래스의 static 메서드인 Pattern compile(String regex)을 호출하여 Pattern 인스턴스를 얻는다.
2. 정규식으로 비교할 대상을 매개변수로 Pattern 클래스의 Matcher matcher(CharSequence input)를 호출해서 Matcher 인스턴스를 얻는다.
3. Matcher 인스턴스에 boolean matches()를 호출해서 정규식에 부합하는지 확인한다.

![정규표현식(정규식) - javascript](https://t1.daumcdn.net/cfile/tistory/995A24405A9BD92A3A)

