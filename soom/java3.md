# [Tech Interview]

JVM의 메모리 구조에 대해서 설명해 주세요.
 
 </br>

## JVM 메모리 구조에 대해서 알아야 하는 이유?
JVM을 몰라도 코드 짜고 실행시키는데 큰 문제는 없습니다.

다만 대규모 프로젝트에서는 메모리 관리에 따라 프로그램의 성능이 크게 차이가 날 수 있으므로 제대로 된 JAVA 개발자라면 JVM 메모리 구조에 대해 제대로 알아 둘 필요가 있겠죠?

 </br>
 
## JVM 이란?
JVM은 Java Virtual Machine의 약자로, 자바 가상 머신을 의미합니다.

JVM은 자바와 운영체제 사이에서 중개자 역할을 수행합니다.

또한 자바가 운영체제에 구애 받지 않고 프로그램을 실행할 수 있도록 도와줍니다.

또한 가비지 컬렉터를 이용한 메모리 관리도 자동으로 수행하며, 다른 하드웨어와 다르게 레지스터 기반이 아닌 스택 기반으로 작동합니다. (가비지 컬렉터를 이용한 메모리 관리를 언저 자체적으로, 자동으로 관리하기 때문에 Managed Language라고 부릅니다.)

 

자바 언어의 가장 큰 특징으로 'Write Once and Run Anywhere'을 꼽습니다.

자바 언어로 작성된 코드는 운영체제에 관계없이 어디서든 잘 구동된다는 뜻이죠.

이것이 가능한 이유가 바로 JVM에 있습니다.

![자바 프로그램 실행 단계](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPSafI%2Fbtrr2s3K1hP%2FzI8IM2CKz4YUE2SxhNx6LK%2Fimg.png){ width: 200px; }


 </br>

## 자바 프로그램 실행 단계

자바 코드로 작성된 자바 소스 파일은 바이트 코드로 변환됩니다. 


바이트 코드를 JVM에서 읽어 들인 후, 일련의 과정을 거쳐 운영체제에 관계없이 프로그램이 실행될 수 있게 만드는 것입니다.

 

여기서 이 일련의 과정에 해당하는 부분을 오늘 알아보겠습니다!

 <br>

## JVM 메모리 구조?

위의 그림을 보다 상세히 그려보았습니다.
![JVM 메모리 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbsyPFt%2Fbtrr7qqjzaZ%2Fn2uTTL61ybjuTjX1f0fU70%2Fimg.png)

자바 소스 파일은 자바 컴파일러에 의해서 바이트 코드 형태인 클래스 파일이 됩니다.

그리고 이 클래스 파일은 클래스 로더가 읽어들이면서 JVM이 수행됩니다.
 

<br>
 
### (1) Class Loader
Class Loader는 JVM 내로 클래스 파일을 로드하고, 링크를 통해 Runtime Data Area에 배치하는 작업을 수행하는 모듈입니다.

런타임 시에 동적으로 클래스를 로드합니다.

<br>


### (2) Runtime Data Area

Class Loader를 통해서 배치된 메모리들은 Runtime Data Area에 배치되는데, Runtime Data 영역은 JVM의 메모리 영역입니다. 

자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역입니다. 

이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있습니다.


![런타임 데이터 영역](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fs7FsC%2Fbtrr16tmFwQ%2FK1LTiwgA5mClkwlVRIqcr0%2Fimg.png)


#### (2-1) 클래스 영역

모든 쓰레드가 공유하는 메모리 영역입니다.

클래스 영역은 클래스, 인터페이스, 메소드, 필드, Static 변수 등의 바이트 코드를 보관합니다.

 

#### (2-2) 스택 영역


메서드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성됩니다.

그리고 메서드 안에서 사용되는 값들을 저장하고,

호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장합니다.

마지막으로, 메서드 수행이 끝나면 프레임별로 삭제합니다.

힙 영역에 도달하기 위한 주소값도 스택 영역에 보관이된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk5BgJ%2Fbtrr3jL4wXV%2Fzby6BnFw2oUC1AOqR9N4YK%2Fimg.png)

 

#### (2-3) Heap area 

모든 쓰레드가 공유하며, new 키워드로 생성된 객체와 배열이 생성되는 영역입니다.

또한, 메소드 영역에 로드된 클래스만 생성이 가능하고

Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역입니다.

 

#### (2-4) Native method stack

자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역입니다.



 

#### (2-5) PC Register

쓰레드가 시작될 때 생성되며, 생성될 때마다 생성되는 공간으로 쓰레드마다 하나씩 존재합니다.

쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 하는 부분으로

현재 수행중인 JVM 명령의 주소를 갖습니다.

 


### (3) Execution Engine

Execution Engine은 Class Loader를 통해 JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명렁어 단위로 읽어서 실행합니다.

최초 JVM이 나왔을 당시에는 인터프리터 방식이었기때문에 속도가 느리다는 단점이 있었지만

JIT 컴파일러 방식을 통해 이 점을 보완하였습니다.


JIT 컴파일러 방식은, 변환 작업은 인터프리터에 의해서 지속적으로 수행되지만, 
이미 한번 읽어서 기계어로 변경한, 소스코드는 다시 번역하지 않도록 
캐시에 담아두었다가(메모리에 올려두었다가) 재사용합니다.

그래서 실행이 빠르다는 장점이 있지만, 최초에 번역할 때 시간이 많이 걸린다는 단점이 있습니다.

<br>

### (3) Garbage Collector

Garbage Collector(GC)는 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거하는 역할을 합니다. 이때, GC가 역할을 하는 시간은 언제인지 정확히 알 수 없습니다.

https://github.com/isoomni/Tech-Interview/blob/main/soom/java_GC_eun.md

Code 	실행할 코드(프로그램)
Data 	전역변수, 정적변수
Heap	사용자 동적할당 (런타임에 결정)
Stack	지역변수, 매개변수

<br>
