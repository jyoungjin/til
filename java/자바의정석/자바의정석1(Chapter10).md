# Java의 정석 2 (Chapter 10~12)

### 2021/07/21~

----

## Chapter 10. 날짜와 시간 & 형식화

#### 형식화 클래스

1. DecimalFormat

```java
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number);
```



| 기호   | 의미                     | 패턴                       | 결과                                                   |
| ------ | ------------------------ | -------------------------- | ------------------------------------------------------ |
| 0      | 10진수(값이 없을 때는 0) | 0<br />0.0<br />0000.000   | 123456<br />123456.7<br />0123.450                     |
| #      | 10진수                   | #<br />#.#<br />#####.#### | 123456123456<br />123456.7<br />1234567.89             |
| .      | 소수점                   | #.#                        | 1234567.9                                              |
| -      | 음수부호                 | #.#-<br />-#.#             | 1234567.9-<br />-1234567.9                             |
| ,      | 단위 구분자              | #,###.##<br />#,####.##    | 1,234,567.89<br />123,4567.89                          |
| E      | 지수기호                 | #E0<br />0E0               | .1E7<br />1E6                                          |
| ;      | 패턴구분자               | #,###.##+;#,###.##-        | 1,234,567.89+(양수일 때)<br />1,234,567.89-(음수일 때) |
| %      | 퍼센트                   | #.#%                       | 123456789%                                             |
| \u2030 | 퍼밀                     | #.#\u2030                  | 1234567890‰                                            |
| \u00A4 | 통화                     | \u00A4 #,###               | ₩ 1,234,568                                            |
| `      | escape 문자              | `#`#,###                   | #1,234,568                                             |



2. SimpleDateFormat

```java
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
String result = df.format(today);
```



| 기호 | 의미                                   | 보기            |
| ---- | -------------------------------------- | --------------- |
| G    | 연대(BC, AD)                           | AD              |
| y    | 년도                                   | 2006            |
| M    | 월(1~12 or 1월~12월)                   | 10 or 10월, OCT |
| w    | 년의 몇 번째 주(1~53)                  | 50              |
| W    | 월의 몇 번째 주(1~5)                   | 4               |
| D    | 년의 몇 번째 일(1~366)                 | 100             |
| d    | 월의 몇 번째 일(1~31)                  | 15              |
| F    | 월의 몇 번째 요일(1~5)                 | 1               |
| E    | 요일                                   | 월              |
| a    | 오전/오후(AM,PM)                       | PM              |
| H    | 시간(0~23)                             | 20              |
| k    | 시간(1~24)                             | 13              |
| K    | 시간(0~11)                             | 10              |
| h    | 시간(1~12)                             | 11              |
| m    | 분(0~59)                               | 35              |
| s    | 초(0~59)                               | 55              |
| S    | 천분의 일초(0~999)                     | 253             |
| z    | Time zone(General time zone)           | GMT+9:00        |
| Z    | Time zone(RFC 822 time zone)           | +0900           |
| `    | escape문자(특수문자를 표현하는데 사용) | 없음            |



3. ChoiceFormat

```java
import java.text.*;

class Main {
  public static void main(String[] args){
		double[] limits = {60, 70, 80, 90}; // 낮은 값부터 큰 값의 순서로 적어야한다.
    String[] grades = {"D", "C", "B", "A"}; // limits, grades 간의 순서와 개수를 맞추어야 한다.
    int[] scores = {100,95,88,70,52,60,70};
    
    ChoiceFormat form = new ChoiceFormat(limits, grades);
    
    for(int i=0;i<scores.length;i++){
      System.out.println(scores[i]+":"+form.format(scores[i]));
    }
  }
}
```



4. MessageFormat

```java
import java.text.*;

class Main {
  public static void main(String[] args){
		String msg = "Name: {0} \nTel: {1} \nAge: {2}\nBirthday: {3}";
    
    Object[] arguments = {
      "장영진", "010-4432-6011", "26", "01-11"
    }
    
    String result = MessageFormat.format(msg, arguments);
    System.out.println(result);
  }
}
```



#### java.time 패키지

| 패키지             | 설명                                                    |
| ------------------ | ------------------------------------------------------- |
| java.time          | 날짜와 시간을 다루는데 필요한 핵심 클래스들을 제공      |
| java.time.chrono   | 표준(ISO)이 아닌 달력 시스템을 위한 클래스들을 제공     |
| java.time.format   | 날짜와 시간을 파싱하고, 형식화하기 위한 클래스들을 제공 |
| java.time.temporal | 날짜와 시간의 필드와 단위를 위한 클래스들을 제공        |
| java.time.zone     | 시간대와 관련된 클래스들을 제공                         |



- 시간을 표현할 때: LocalTime
- 날짜 & 시간을 표현할 때: LocalDateTime
- 날짜 & 시간 & 시간대를 표현할 때: ZonedDateTime

유용한 메서드

- 객체 생성 - now(), of()
- 필드 값 변경하기 - with(), plus(), minus()
- 날짜와 시간의 비교 - isAfter(), isBefore(), isEqual()

----

## Chapter 11. 컬렉션 프레임워크

#### 형식화 클래스

1. DecimalFormat
