# [Tech Interview]

Java 8 버전에 추가된 중요 기능들에 대하여서 설명해주세요.

 </br>


## Java 8 버전에 추가된 중요 기능

Java 8에서 추가되거나 변경된 것들은 매우 많습니다.
그 중에서 중요한 것은 다음과 같습니다.

+ Lamda 표현식
+ Function(함수형) 인터페이스
+ Stream
+ Optional
+ 인터페이스의 기본 메소드 Default Method
+ 날짜 관련 클래스 추가
+ 병렬 배열 정렬
+ StringJoiner 추가

</br>

---

</br>

### 1. Optional

</br>

객체를 편리하게 처리하기 위해서 만든 클래스이입니다.
Optional 클래스는 java.util 패키지에 속해 있습니다.

```java
public final class Optional<T>
    extends Object
```
위 클래스는 Object 클래스를 확장했고, final 클래스로 선언되어 있고, Generic한 클래스입니다.
final 변수는 변경이 불가능하지만, final 클래스로 선언했다고 해서 내용 변경이 불가능한 것은 아닙니다. 대신 추가적인 확장은 불가능합니다. 즉, 자식 클래스를 만들 수 없다는 것이죠.

</br>
Optional 클래스에 대해서 이해하려면, Optional 클래스가 하나의 깡통이라고 생각하면 됩니다.
깡통에는 물건을 넣을 수도 있고, 아무 물건이 없을 수도 있습니다.

```java
new Optional)();
```

Optional 클래스는 위와 같이 객체를 생성하지 않습니다.
그냥,

```java
Optional.empty(), Optional.of(), Optional.ofNullable()
```
메소드를 통해서 Optional 클래스를 리턴하면 됩니다.

</br>

1) 데이터가 없는 Optional 객체를 생성하기 위해서는 empty(); 메소드를 사용합니다.

2) 만약 null이 추가될 수 있는 상황이라면 ofNullable(common); 메소드를 사용합니다.

3) 반드시 데이터가 들어갈 수 있는 상황에는 of() 메소드를 사용합니다.

4) Optional 클래스가 비어 있는지 확인하기 위해서는 isPresent() 메소드를 사용합니다.  Optional.ofNullable().isPresent();

</br>

Optional 클래스는 null 처리를 보다 간편하게 하기 위해서 만들어졌습니다.

자칫 잘못하면 NullPointerException이 발생할 수도 잇는데, 이 문제를 보다 간편하고 명확하게 처리하려면 Optional을 사용하면 됩니다.

</br></br>

### 2. Default method

Default method 는 하위 호환성을 이유로 만들었습니다.

java 8 이전에 interface는 오직 public abstract method만 가질 수 있었습니다. 

그렇기 때문에 인터페이스에 하나의 메서드를 추가하려면, 그 인터페이스를 구현한 모든 클래스에 그 메서드를 추가한 후 구현도 해줘야했습니다.

하지만 default method가 생기면서 그것을 상속한 자식 클래스에 영향을 미치지 않도록 할 수 있게 되었습니다. 

예를 들어 오픈소스를 만들었다고 가정하겠습니다.
그 오픈 소스가 엄청 유명햊서 전 세계 사람들이 다 사용하고 있는데, 인터페이스에 새로운 메소드를 만들어야 한다면, 내가 만든 오픈 소스를 사용한 사람들이 모두 오류가 발생하고 수정해야 하는 일이 발생합니다.

이럴 때 사용하는 것이 default 메소드 입니다.


</br></br>

### 3. 날짜 관련 클래스들
이전에는 Date, SimpleDateFormatter 라는 클래스를 사용하여 날짜를 처리해왔습니다.

하지만 이 클래스들은 쓰레드에 안전하지 않습니다.

Java 8에서는 java.time이라는 패키지를 만들었습니다.

</br>

<table>
  <tr>
    <th>내용</th>
    <th>버전</th>
    <th>패키지</th>
    <th>설명</th>
  </tr>
  <tr>
  <tr>
    <td rowspan="2">값 유지</td>
    <td>예전버전</td>
    <td>java.util.Date</br>java.util.Calendar</td>
    <td>Date 클래스는 날짜 계산을 할 수 없다. Calendar 클래스는 불변 객체가 아니므로 연산 시 객체 자체가 변경되었다.</td>
  </tr>
  <tr>
    <td>Java8</td>
    <td>java.time.ZonedDateTime</br>java.time.LocalDate</td>
    <td>두 객체는 불변 객체이다. 모든 클래스가 연산용의 메소드를 갖고 있으며, 연산 시 새로운 불변 객체를 돌려 준다. 쓰레드에 안전하다.</td>
  </tr>

  <tr>
    <td rowspan="2">변경</td>
    <td>예전버전</td>
    <td>java.text.SimpleDateFormat</td>
    <td>쓰레드에 안전하지 않고 느리다.</td>
  </tr>
  <tr>
    <td>Java8</td>
    <td>java.time.format.DateTimeFormatter</td>
    <td>쓰레드에 안전하며 빠르다.</td>
  </tr>

  <tr>
    <td rowspan="2">시간대</td>
    <td>예전버전</td>
    <td>java.time.ZoneId</br>java.time.ZoneOffSet</td>
    <td>"Asia/Seoul"이나 "+09:00"같은 정보를 가진다.</td>
  </tr>
  <tr>
    <td>Java8</td>
    <td>java.time.ZoneId</br>java.time.ZoneOffSet</td>
    <td>ZoneId는 "Asia/Seoul"를, ZoneOffSet은 "+09:00"같은 정보를 가진다</td>
  </tr>

  <tr>
    <td rowspan="2">속성관련</td>
    <td>예전버전</td>
    <td>java.time.temporal.ChronoField</td>
    <td>ChronoField.YEAR</br>ChronoField.MONTH_OF_YEAR</br>ChronoField.DAY_OF_MONTH</BR>등이 enum 타입이다.</td>
  </tr>
  <tr>
    <td>Java8</td>
    <td>java.time.temporal.TemporalUnit</td>
    <td>TemporalUnit.YEARS</br>TemporalUnit.MONTHS</br>TemporalUnit.DAYS</BR>등이 enum 타입이다.</td>
  </tr>

</table>


### 3. 병렬 배열 정렬

자바에서 배열을 정렬하는 가장 편한 방법은 java.util 패키지의 Arrays 클래스를 사용하는 것입니다.

+ binarySearch() :  배열 내에서의 검색
+ copyOf() : 배열의 복제
+ equals() : 배열의 비교
+ fill() : 배열 채우기
+ hashCode() : 배열의 해시코드 제공
+ sort() : 정렬
+ toString() : 배열 내용을 출력

java 8 에서는 parallelSort()라는 정렬 메소드가 제공됩낟.
sort 대신 정렬할 때 사용할 수 있는데, 5,000개 부터 parallelSort()의 성능이 더 빠릅니다.

</br></br>

### 4. StringJoiner

기존에 문자열을 처리하는 방법에 String, StringBuilder, StringBuffer 등이 있었습니다.

java 8에서는 StringJoiner 라는 클래스가 새롭게 추가되었습니다.

순차적으로 나열되는 문자열을 처리할 때 사용합니다.

</br></br>

### 5. 람다
 람다식(Lambda expression)은 간단히 말해서 메서드를 하나의 식으로 표현한 것입니다.

</br></br>


 ### 6. Optional<T>

 자바 8 이전의 개발자들은 NullPointerException(NPE)이 발생할 수 있다는 가능성 때문에, 이에 대한 유효성 검사 및 에러 방지를 위한 많은 코드를 추가해야만 했습니다. 
 
 Optional<T> 클래스는 NPE를 효과적으로 다룰 수 있게해주는 콘테이너 클래스입니다. 
 
 Optional 콘테이너 내부의 객체가 null이 아니라면 그 객체를 반환하고, null일 경우에는 NPE가 발생하는 대신 사전에 정의된 액션을 취합니다.

Optional<T>를 사용하면 코드가 더 간결해지고 직관적으로 변합니다.
