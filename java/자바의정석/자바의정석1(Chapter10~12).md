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

## Chapter 11. Collection 프레임워크

#### Collection 프레임워크

> ​	데이터 군을 저장하는 클래스들을 표준화한 설계 (JDK 1.2 이후)

![images_da__hey_post_339fe41d-d1be-433a-918a-8fca141967c9_image](/Users/youngjin/Desktop/images_da__hey_post_339fe41d-d1be-433a-918a-8fca141967c9_image.png)



| 인터페이스 | 특징                                                         |
| ---------- | ------------------------------------------------------------ |
| List       | 순서가 있는 데이터의 집합. 데이터의 중복을 허용한다.<br />구현클래스: ArrayList, LinkedList, Stack, Vector |
| Set        | 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다.<br />구현클래스: HashSet, TreeSet |
| Map        | 키와 값의 쌍으로 이루어진 데이터의 집합.<br />순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.<br />구현클래스: HashMap, TreeMap, Hashtable, Properties |



- Collction (List와 Set의 조상)

| 메서드                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| boolean add(Object o)<br />boolean add(Collection c)         | 지정된 객체 또는 Collection의 객체들을 Collection에 추가한다. |
| void clear()                                                 | Collection의 모든 객체를 삭제한다.                           |
| boolean contains(Object o)<br />boolean containsAll(Collection c) | 지정된 객체 또는 Collection의 객체들이 Collection에 포함되어 있는지 확인한다. |
| boolean equals(Object o)                                     | 동일한 Collection인지 비교한다.                              |
| int hashCode()                                               | Collection의 hash code를 반환한다.                           |
| boolean isEmpty()                                            | Collection이 비어있는지 확인한다.                            |
| Iterator iterator()                                          | Collection의 iterator를 얻어서 반환한다.                     |
| boolean remove(Object o)                                     | 지정된 객체를 삭제한다.                                      |
| boolean removeAll(Collection c)                              | 지정된 Collection에 포함된 객체들을 삭제한다.                |
| boolean retianAll(Collection c)                              | 지정된 Collection에 포함된 객체만을 남기고 다른 객체들은 Collection에서 삭제한다. <br />이 작업으로 인해 Collection에 변화가 있으면 true, 그렇지 않으면 false를 반환한다. |
| int size()                                                   | Collection에 저장된 객체의 개수를 반환한다.                  |
| Object[] toArray()                                           | Collection에 저장된 객체를 객체배열(Object[])로 반환한다.    |
| Object[] toArray(Object[] a)                                 | 지정된 배열에 Collection의 객체를 저장해서 반환한다.         |



- List 인터페이스: 중복을 허용, 저장순서가 유지

| 메서드                                                       | 설명                                                       |
| ------------------------------------------------------------ | ---------------------------------------------------------- |
| void add(int index, Object element)<br />boolean addAll(int index, Collection c) | 지정된 위치에 객체 또는 컬렉션에 포함된 객체들을 추가한다. |
| Object get(int index)                                        | 지정된 위치에 있는 객체를 반환한다.                        |
| int indexOf(Object o)                                        | 지정된 객체의 위치를 반환한다. (순방향)                    |
| int lastIndexOf(object o)                                    | 지정된 객체의 위치를 반환한다. (역방향)                    |
| ListIterator listIterator()<br />ListIterator listIterator(int index) | List의 객체에 접근할 수 있는 ListIterator를 반환한다.      |
| Object remove(int index)                                     | 지정된 위치에 있는 객체를 삭제하고 삭제된 객체를 반환한다. |
| Object set(int index, Object element)                        | 지정된 위치에 객체를 저장한다.                             |
| void sort(Comparator c)                                      | 지정된 비교자로 List를 정렬한다.                           |
| List subList(int fromIndex, int toIndex)                     | 지정된 범위에 있는 객체를 반환한다.                        |



- Set 인터페이스: 중복을 허용하지 않음, 저장순서가 유지되지 않는다.

- Map 인터페이스: key는 중복을 허용하지 않음, value는 중복 허용

| 메서드                               | 설명                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| void clear()                         | Map의 모든 객체를 삭제한다.                                  |
| boolean containsKey(Object key)      | 지정된 key객체와 일치하는 Map의 key객체가 있는지 확인한다.   |
| boolean containsValue(Object value)  | 지정된 value객체와 일치하는 Map의 value객체가 있는지 확인한다. |
| Set entrySet()                       | Map에 저장되어 있는 key-value쌍을 Map.Entry 타입의 객체로 저장한 Set으로 반환한다. |
| boolean equals(Object o)             | 동일한 Map인지 비교한다.                                     |
| Object get(Object key)               | 지정한 key객체에 대응하는 value객체를 찾아서 반환한다.       |
| int hashCode()                       | 해시코드를 반환한다.                                         |
| boolean isEmpty()                    | Map이 비어있는지 확인한다.                                   |
| Set keySet()                         | Map에 저장된 모든 key객체를 반환한다.                        |
| Object put(Object key, Object value) | Map에 value객체를 key객체에 연결하여 저장한다.               |
| void putAll(Map t)                   | 지정된 Map의 모든 key-value쌍을 추가한다.                    |
| Object remove(Object key)            | 지정한 key객체와 일치하는 key-value객체를 삭제한다.          |
| int size()                           | Map에 저장된 key-value쌍의 개수를 반환한다.                  |
| Collection values()                  | Map에 저장된 모든 value객체를 반환한다.                      |

- Map.Entry 인터페이스

| 메서드                        | 설명                                      |
| ----------------------------- | ----------------------------------------- |
| boolean equals(Object o)      | 동일한 Entry인지 비교한다.                |
| Object getKey()               | Entry의 key객체를 반환한다.               |
| Object getValue()             | Entry의 value객체를 반환한다.             |
| int hashCode()                | Entry의 해시코드를 반환한다.              |
| Object setValue(Object value) | Entry의 value객체를 지정된 객체로 바꾼다. |

----

- ArrayList

  - List 인터페이스를 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용한다.
  - 기존의 Vector를 개선한 것으로 구현원리와 기능적인 측면에서 동일하다.
  - 배열에 더 이상 저장할 공간이 없으면 보다 큰 벼열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장된다.(기본크기:10)
  - 삭제할 데이터의 아래에 있는 데이터를 한 칸씩 위로 복사해서 삭제할 데이터를 덮어쓴다.

- LinkedList

  - 배열의 단점을 보완하기 위해 만들어진 자료구조로 불연속적인 데이터를 서로 연결한 형태로 구성되어 있다.
    - 배열의 단점 1) 크기를 변경할 수 없다.
    - 배열의 단점 2) 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.
  - 삭제하고자 하는 요소의 이전요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하기만 하면 된다.
  - 실제로 이름과 달리 LinkedList는 더블 링크드 리스트(양방향)로 구현되어 있다.

- 결론

  - 순차적으로 추가/삭제하는 경우에는 ArrayList가 LinkedList보다 빠르다.
  - 중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠르다.

  | 컬렉션     | 읽기(접근 시간) | 추가/삭제 | 비고                                                    |
  | ---------- | --------------- | --------- | ------------------------------------------------------- |
  | ArrayList  | 빠르다          | 느리다    | 순차적인 추가삭제는 더 빠름.<br />비효율적인 메모리사용 |
  | LinkedList | 느리다          | 빠르다    | 데이터가 많을수록 접근성이 떨어짐.                      |

  
