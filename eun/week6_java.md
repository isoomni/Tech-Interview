## Java Reflection이란 무엇이고, 어떨 때 사용되는 것인가요?
#### week6 - 2022-05-14


### Java Reflection이란?

- 구체적인 클래스 타입을 알지 못하더라도 그 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API
- 컴파일 시간이 아닌 실행 시간에 동적으로 특정 클래스의 정보를 추출할 수 있는 프로그래밍 기법

### Java Reflection은 언제 사용될까?
- 동적으로 클래스를 사용해야할 때
  - 작성 시점에는 어떠한 클래스를 사용해야 할지 모르지만 런타임 시점에서 클래스를 가져와서 실행해야하는 경우
- Spring 프레임워크의 어노테이션 같은 기능들이 리플렉션을 이용하여 프로그램 실행 도중 동적으로 클래스의 정보를 가져와서 사용함.


### 사용 방법

- reflection을 사용하기 위해서는 3개의 클래스를 import 해야함.
   - `java.lang.reflect.Method`
   - `java.lang.reflect.Field`
   - `java.lang.reflect.Constructor`

- class를 찾기 위한 `java.lang.Class`는 import하지 않아도 됨.

### 1️⃣ Class 찾기

```java
    Class clazz = Child.class; // 클래스 정보 할당
    System.out.println("Class name: " + clazz.getName()); // .getname() -> 클래스 이름 리턴

    // output
    // Class name: test.Child
```
위의 예시는 IDE에서 클래스를 알고 있을 때 사용 가능  
<br>

만약 클래스를 참조할 수 없고, 이름만 알고 있다면? -> `Class.forName()`

```java
    Class clazz2 = Class.forName("test.Child");
    System.out.println("Class name: " + clazz2.getName());

    // output
    // Class name: test.Child

```

### 2️⃣ Constructor 찾기

- 클래스로부터 생성자를 가져오기
- 인자 없는 생성자를 가져오기 -> `getDeclaredConstructor()`
- 원하는 타입과 일치하는 생성자 가져오기 -> `getDeclaredConstructor(Param)`
- 모든 생성자 가져오기(private, putblic 등)-> `getDeclaredConstructors()`
- public 생성자만 가져오기 -> `getConstructors()`

<br>

```java
    Class clazz = Class.forName("test.Child");
    Constructor constructor = clazz.getDeclaredConstructor();
    System.out.println("Constructor: " + constructor.getName());

    // output
    // Constructor: test.Child
```

```java
    Class clazz = Class.forName("test.Child");
    Constructor constructor2 = clazz.getDeclaredConstructor(String.class);
    System.out.println("Constructor(String): " + constructor2.getName());

    // output
    // Constructor(String): test.Child
```

### 3️⃣ Method 찾기

- 이름으로 메소드를 찾기 -> `getDeclaredMethod()`
  - 인자로 메소드의 파라미터 정보를 넘겨주면 일치하는 것을 찾아준다.
  - 인자가 없는 메소드라면 null을 전달
  - 메소드를 찾을 때 존재하지 않는다면 `NoSuchMethodException` 에러가 발생
  - 인자가 두개 이상이라면 클래스 배열을 만들어서 인자를 넣어준다.


- 함수 이름에 Declared가 들어가면 Super 클래스의 정보는 가져오지 않는다.

-  public 메소드를 리턴, 상속받은 메소드들도 모두 찾기 -> `getMethods()`



```java
    Class clazz = Class.forName("test.Child");
    Method method1 = clazz.getDeclaredMethod("method4", int.class);
    System.out.println("Find out method4 method in Child: " + method1);

    // Find out method4 method in Child: public int test.Child.method4(int)

```

인자 없을 때
```java
    Class clazz = Class.forName("test.Child");
    Method method1 = clazz.getDeclaredMethod("method4", null);
```

인자가 두 개 이상일 때
```java
    Class clazz = Class.forName("test.Child");
    Class partypes[] = new Class[1];
    partypes[0] = int.class;
    Method method = clazz.getDeclaredMethod("method4", partypes);
```

### 4️⃣ Field(변수) 찾기

- 전달된 이름과 일치하는 Field를 찾기 -> `getDeclaredField()`
- 객체에 선언된 모든 Field를 찾기 -> `getDeclaredFields()`
- 상속받은 클래스를 포함한 public Field를 찾기 -> `getFields()`

```java
    Class clazz = Class.forName("test.Child");
    Field field = clazz.getDeclaredField("cstr1");
    System.out.println("Find out cstr1 field in Child: " + field);

    // output
    // Find out cstr1 field in Child: public java.lang.String test.Child.cstr1

```


### 5️⃣ Field 변경
- 클래스로부터 변수 정보를 가져와 객체의 변수를 변경
- private 변수를 수정하려면 `setAccessible(true)`로 접근 상태를 변경하면 된다.
  

```java
    Child child = new Child();
    Class clazz = Class.forName("test.Child");
    Field fld = clazz.getField("cstr1");
    System.out.println("child.cstr1: " + fld.get(child));

    fld.set(child, "cstr1");
    System.out.println("child.cstr1: " + fld.get(child));

    // output
    // child.cstr1: 1
    // child.cstr1: cstr1
```


```java
    Child child = new Child();
    Class clazz = Class.forName("test.Child");
    Field fld2 = clazz.getDeclaredField("cstr2");
    fld2.setAccessible(true); // 접근 상태 변경
    fld2.set(child, "cstr2");
    System.out.println("child.cstr2: " + fld2.get(child));

    // output
    // child.cstr2: cstr2
```

---
참고사이트  
[Java - Reflection 쉽고 빠르게 이해하기](https://codechacha.com/ko/reflection/)  
[JAVA - 리플렉션(Reflection)이란?](https://kdg-is.tistory.com/entry/JAVA-%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98-Reflection%EC%9D%B4%EB%9E%80)
