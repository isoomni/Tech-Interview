## JVM은 어떤 방식으로 코드를 해석하고 실행시키는지 흐름에 맞게 설명해 주세요. (Java 실행 흐름)
#### week1 - 2022-04-07

### JVM이란?

- **J**ava **V**irtual **M**achine(자바 가상 머신)의 약자

- 자바와 운영체제 사이에서 <u>중개자 역할</u>을 수행

- 자바가 <u>운영체제에 종속되지 않고</u> 프로그램을 실행할 수 있도록 도와준다.

- Garbage Collector를 사용한 메모리 관리도 자동으로 수행한다.

- 레지스터 기반이 아닌 스택 기반으로 동작

<br> 

### 자바 프로그램 실행 단계

![java](/eun/image/week1_java1.jpg)


1. 자바 컴파일러에 의해 자바 소스파일 → 바이트 코드(.`class` 파일)로 변환
2. 바이트 코드를 JVM에서 읽어 들인 다음에 어떤 운영체제 든 간에 프로그램을 실행할 수 있도록 함 알맞는 기계어로 변환 ⇒ 이러한 특징을 `WriteOnce, RunAnywhere(WORA)` 또는 `WriteOnce, RunEveryWhere(WORE)`라고 함.

- 만약 자바 소스 파일은 리눅스에서 만들었고, 윈도우에서 파일을 실행하고 싶다면→ 윈도우용 JVM을 설치하면 됨.
- JVM은 운영체제에 종속적임을 알 수 있다.

- ByteCode는 JVM이 이해할 수 있는 언어로 변환된 `.class` 파일
  - `.class` 파일들은 `.java` 파일로 역컴파일 가능
<br>  

### JVM의 구체적인 동작 과정

1. JVM은 운영체제로부터 프로그램이 필요로하는 **메모리를 할당**받음. (JVM은 메모리를 용도에 따라 여러 영역으로 나누어 관리)
2. 자바 컴파일러가 자바 소스 코드를 읽어들여 자바 **바이트코드(`.class`)로 변환**시킴
3. Class Loader를 통해 class 파일들을 **JVM으로 로딩**
4. 로딩된 class 파일들은 Execution engine을 통해 **해석**됨
5. 해석된 바이트코드는 Runtine Data Area에 배치되어 수행이 이루어짐
6. 실행 과정 속에서 JVM은 필요에 따라 Thread Synchronization과 Garbage Collector같은 관리 작업 수행

### JVM 메모리 구조

![java](/eun/image/week1_java2.png)

JVM의 구조는 크게 보면 4가지로 나눌 수 있음 → Class Loader, Execution Engine, Garbage Collector, Runtime Data Area

#### Class Loader
- JVM 내로 **클래스 파일(`.class`)을 로드**하고 링크를 통해 배치하는 작업 수행
- 런타임 시에 동적으로 클래스를 로드
- jar 파일 내 저장된 클래스들을 JVM 내의 Runtime Data Area에 배치
   
#### Execution Engine
- Class Loader를 통해 JVM 내의 Runtime Data Area에 배치된 **바이트코드를 해석**하여 JVM에서 실행가능하도록 함.
- 최초 JVM이 나왔을 당시에는 인터프리터 방식이었기 때문에 속도가 느리다는 단점 → JIT 컴파일러 방식을 통해 보완
  - 인터프리터 : 자바 바이트코드를 명령어 단위로 읽고 해석하는 역할
  - JIT 컴파일러 : 자바 바이트코드를 기계어로 변환시키는 역할
- JIT는 바이트 코드를 어셈블러 같은 네이티브 코드로 바꿈으로써 실행이 빠르지만, 변환하는 데 비용이 발생. ⇒ 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준이 넘어가면 JIT 컴파일러 방식으로 실행
   
#### Garbage Collector(GC)
- 힙 메모리 영역에 생성된 객체들 중 **참조되지 않은 객체들을 탐색 후 제거**

#### Runtime Data Area
- JVM의 **메모리 영역**
- 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역
- Method Area, Heap Area, Stack Area, PC Register, Native Method Stack으로 나눌 수 있음
        
![java](/eun/image/week1_java3.png)
        
1. Method Area
- 모든 쓰레드가 공유하는 메모리 영역.
- 메소드 영역은 클래스, 인터페이스, 메소드, 필드, Static 변수 등의 **바이트 코드를 보관**
2.  HeapArea
- 모든 쓰레드가 공유
- **new 키워드로 생성된 객체와 배열**이 생성되는 영역
- 메소드 영역에 로드된 클래스만 생성 가능
- Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역
3.  Stack Area
- 메서드 호출 시마다 각각의 스택 프레임(그 메서드 만을 위한 공간)이 생성
- **메서드 안에서 사용되는 값**들을 저장하고, 호출된 메서드의 매개변수, 지역 변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장.
- 메서드 수행이 끝나면 프레임별로 삭제
4.  PC Register
- 쓰레드가 시작될 때 생성
- 생성 될 때마다 생성되는 공간으로 쓰레드 마다 하나씩 존재
- 쓰레드가 어떤 부분을 무슨 명령으로 실행해야 할 지에 대한 기록을 하는 부분
- 현재 수행중인 JVM 명령의 주소를 가짐
5.  Native method stack
- 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역

---
참고사이트  
[JVM 메모리 구조란? (JAVA)](https://steady-coding.tistory.com/305)  
[JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.](https://velog.io/@kwj1270/JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80)