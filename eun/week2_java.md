## Garbage Collector은 무엇이고, Parallel GC와 CMS GC, G1 GC의 큰 차이는 무엇인지 설명해주세요. (mark-sweep-compact, concurrency-sweep, garbage-first)
#### week2 - 2022-04-15

### Garbage Collector란?
- 크게 다음 2가지 작업을 수행한다
1. 힙(heap) 내의 객체 중에서 가비지(garbage)를 찾아낸다.
2. 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다.
- 최초의 Java에서는 이들 Garbage Collection(GC) 작업에 애플리케이션의 사용자 코드가 관여하지 않도록 구현되어 있었다.
- 그러나 위 2가지 작업에서 좀 더 다양한 방법으로 객체를 처리하려는 요구 -> JDK 1.2부터는 java.lang.ref 패키지를 추가해 제한적이나마 사용자 코드와 GC가 상호작용할 수 있다.


### Garbage란? 
- 정리되지 않은 메모리, 유효하지 않은 메모리 주소
- 주소를 잃어버려서 사용할 수 없는 메모리 -> '정리 되지 않은 메모리'
- 자바에서는 `Garbage`라고 부른다.
- 앞으로 사용하지 않고 메모리를 가지고 있는 객체도 `Garbage`이다.


### GC (Garbage Collecton)
- 더이상 사용하지 않는 객체 등을 메모리에서 해제(삭제)하는 JVM의 작업
- Java 프로세스가 동작하는 과정에서 GC는 불필요한 또는 더이상은 사용하지 않는 객체들을 메모리에서 제거함으로써, Java 프로세스가 한정된 메모리를 효율적으로 사용할 수 있게 해준다.
- JVM에서 GC의 스케줄링을 담당함으로서 Java 프로그래머들에게는 메모리를 관리해야하는 부담을 줄여준다.




### Java 에서 GC를 도입이 가능했던 이유
1. 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

=> 이러한 가설을 `weak generational hypothesis`라 한다. 

### GC 과정

#### Stop-the-World 
-GC 실행을 위해 JVM이 애플리케이션 실행을 멈추는 것.
- GC가 실행될 때는, GC를 실행하는 쓰레드를 제외한 모든 스레드들이 작업을 멈춤. GC 작업이 완료된 후 중단했던 작업을 다시 시작함.
- 대개의 경우 GC 튜닝이란 stop-the-world 시간을 줄이는 것을 말한다.



### Mark and Sweep
- GC의 과정을 Mark and Sweep이라고도 한다. 
- GC가 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾는 과정이 Mark라고 한다. 이 과정에서 Stop the world가 발생한다. 이후 Mark 되어있지 않은 객체들을 힙에서 제거하는 과정이 Sweep이다.

[Garbage 판별 과정]

- GC는 객체가 가비지인지 판별하기 위해서 reachability라는 개념을 사용
- 어떤 객체에 유효한 참조가 있으면 -> `reachable`
- 유효한 참조가 없으면 -> `unreachable`
- `unreachable` 객체를 가비지로 간주해 GC를 수행
- 객체들은 참조 사슬을 이루며, 유효한 참조 여부를 파악하려면 항상 유효한 최초의 참조가 있어야 하는데 이를 객체 참조의 root set이라고 한다.


힙에 있는 객체들에 대한 참조는 다음 4가지 종류가 있다.(그림참조)
![java](/eun/image/week2_java1.jpg)


### GC Root가 될 수 있는 대상은?
1. JVM 메모리의 Stack 영역(정적 메모리 할당이 이루어지는 장소로 힙 영역에 동적 할당된 값들(Object)에 대한 참조를 얻을 수 있다.)에 존재하는 참조 변수
2. Method Area의 static 데이터
3. JNI에 의해 생성된 객체들
> **JNI**  
> 자바 네이티브 인터페이스(Java Native Interface, JNI)  
> 자바 가상 머신(JVM)위에서 실행되고 있는 자바코드가 네이티브 응용 프로그램(하드웨어와 운영 체제 플랫폼에 종속된 프로그램들)  
> C, C++ 그리고 어샘블리 같은 다른 언어들로 작성된 라이브러리들을 호출하거나 반대로 호출되는 것을 가능하게 하는 프로그래밍 프레임워크

![java](/eun/image/week2_java2.jpg)


#### GC 동작 순서

- 처음 생성된 객체는 시간의 흐름에 따라 아래의 그림과 같이 변한다.
![java](/eun/image/week2_java3.jpg)

처음 생성된 객체`new Model();` Young Generation의 Eden 영역에 위치하게 된다.
Young 영역을 자세히 보면 다음과 같다.

#### Young 영역(Yong Generation 영역)
- 새롭게 생성한 객체의 대부분이 여기에 위치
- 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young 영역에 생성되었다가 사라진다.
- 이 영역에서 객체가 사라지는 것 ->  `Minor GC`

Young 영역은 세 부분으로 나뉜다.
- Eden 영역
- Survivor 영역(2개)

각 영역의 처리 절차
1. 새로 생성한 대부분의 객체는 Eden 영역에 위치
2. Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동
3. Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓인다.
4. 하나의 Survivor 영역이 가득 차게 되면 그 중에서 살아남은 객체를 다른 Survivor 영역으로 이동 -> 그리고 가득 찬 Survivor 영역은 아무 데이터도 없는 상태가 됨.
5. 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 Old 영역으로 이동
- Survivor 영역 중 하나는 반드시 비어 있는 상태로 남아 있어야 한다. 
- 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 두 영역 모두 사용량이 0이라면 여러분의 시스템은 정상적인 상황이 아니다


> Young Generation 영역에서 오래동안 살아남은 객체는 Old Generation 영역으로 옮겨지는데, 오래되었다는 기준은 무엇일까?
- Young Generation 영역에서 Minor GC 가 발생하는 동안 얼마나 오래 살아남았는지로 판단
- 각 객체는 Minor GC에서 살아남은 횟수를 기록하는 age bit 를 가지고 있으며, Minor GC가 발생할 때마다 age bit 값은 1씩 증가 하게되며, age bit 값이 MaxTenuringThreshold 라는 설정값을 초과하게 되는 경우 Old Generation 영역을 객체가 이동 되는 것이다. 
- 또는 Age bit가 MaxTenuringThreshold 초과하기 전이라도 Survivor 영역의 메모리가 부족할 경우에는 미리 Old Generation 으로 객체가 옮겨질 수도 있다.

#### Old 영역(Old Generation 영역)
- 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다. 
- 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다. 
- 이 영역에서 객체가 사라지는 것 ->`Major GC`(혹은 `Full GC`)

- Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다.
- GC 방식에 따라서 처리 절차 달라진다. 
- GC 방식은 JDK 7을 기준으로 5가지 방식이 있다.

> Serial GC  
> Parallel GC  
> Parallel Old GC(Parallel Compacting GC)  
> Concurrent Mark & Sweep GC(이하 CMS)  
> G1(Garbage First) GC  

<br>

### Serial GC
- Young 영역 : Mark Sweep
- Old 영역 : Mark-Sweep-Compact 알고리즘
- Mark-Sweep-Compact 알고리즘
  - 기존의 Mark-Sweep에 Compact 작업 추가
  - Compact : Heap 영역을 정리하기 위한 단계.
  - Old 영역에 살아있는 객체를 식별(Mark)하고 힙의 앞 부분부터 확인하여 살아 있는 것만 남긴다(Sweep). 마지막 단계에서는 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 존재하지 않는 부분으로 나눈다(Compaction).

- Serial GC는 서버의 CPU 코어가 1개일 때 사용하기 위해 개발됨.
- 모든 가비지 컬렉션 일을 처리하기 위해 1개의 쓰레드만을 이용  
=> CPU의 코어가 여러 개인 운영 서버에서 Serial GC를 사용하는 것은 반드시 피해야 한다.


### Parallel GC
- Parallel GC는 Throughput GC라고도 함.
- 기본적인 처리 과정은 Serial GC와 같음.
- 하지만 Parallel GC는 여러 개의 쓰레드를 통해 Parallel하게 GC를 수행 -> GC의 오버헤드를 상당히 줄여준다. 
- 메모리가 충분하고 코어의 개수가 많을 때 유리함.

### Parallel Old GC
- Parallel GC와 Old 영역의 GC 알고리즘만 다르다. 
- Parallel Old GC : Mark-Summary-Compaction이 사용
- Summary 단계 : 앞서 GC를 수행한 영역에 대해서 별도로 살아있는 객체를 식별


### CMS(Concurrent Mark Sweep) GC
- Parallel GC와 마찬가지로 여러 개의 쓰레드를 이용
- 하지만 기존의 Serial GC나 Parallel GC와는 다르게 Mark Sweep 알고리즘을 Concurrent하게 수행하게 된다.

 ![java](/eun/image/week2_java4.png)

- 초기 Initial Mark 단계 : 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝낸다. -> 멈추는 시간은 매우 짧다.
- Concurrent Mark 단계 : 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인. 다른 스레드가 실행 중인 상태에서 동시에 진행
- Remark 단계 : Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인. 
- Concurrent Sweep 단계 : 쓰레기를 정리하는 작업. 다른 스레드가 실행되고 있는 상황에서 진행

=> stop-the-world 시간이 매우 짧다. 모든 애플리케이션의 응답 속도가 매우 중요할 때 CMS GC를 사용하며, Low Latency GC라고도 부른다.

- 그런데 CMS GC는 stop-the-world 시간이 짧다는 장점에 반해 다음과 같은 단점이 존재한다.
- [단점]
  - 다른 GC 방식보다 메모리와 CPU를 더 많이 사용한다.
  - Compaction 단계가 기본적으로 제공되지 않는다.
  - 시스템이 장기적으로 운영되다가 조각난 메모리들이 많아 Compaction 단계가 수행되면 오히려 Stop The World 시간이 길어지는 문제가 발생할 수 있다.

### G1(Garbage First) GC
- G1(Garbage First) GC는 장기적으로 많은 문제를 일으킬 수 있는 CMS GC를 대체하기 위해 개발되었고, Java7부터 지원되기 시작하였다.

- 기존의 GC 알고리즘 : Heap 영역을 물리적으로 Young 영역(Eden 영역과 2개의 Survivor 영역)과 Old 영역으로 나누어 사용
- G1 GC : Eden 영역에 할당하고, Survivor로 카피하는 등의 과정을 사용하지만 물리적으로 메모리 공간을 나누지 않는다. 
- 대신 Region(지역)이라는 개념을 새로 도입하여 Heap을 균등하게 여러 개의 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분하여(Eden 지역인지, Survivor 지역인지, Old 지역인지) 객체를 할당한다.

![java](/eun/image/week2_java5.jpg)

- G1 GC에서는 Eden, Survivor, Old 역할에 더해 `Humongous`와 `Availabe/Unused`라는 2가지 역할을 추가
- Humonguous : Region 크기의 50%를 초과하는 객체를 저장하는 Region
- Availabe/Unused : 사용되지 않은 Region 

- G1 GC의 핵심은 Heap을 동일한 크기의 Region으로 나누고, 가비지가 많은 Region에 대해 우선적으로 GC를 수행하는 것
- G1 GC도 다른 가비지 컬렉션과 마찬가지로 2가지 GC(Minor GC, Major GC)로 나누어 수행된다.
- Minor GC
  - 한 지역에 객체를 할당하다가 해당 지역이 꽉 차면 다른 지역에 객체를 할당 -> Minor GC 실행
  - G1 GC는 각 지역을 추적하고 있기 때문에, 가비지가 가장 많은(Garbage First) 지역을 찾아서 Mark-and-Sweep를 수행
  
  - Eden 지역에서 GC가 수행되면 살아남은 객체를 식별(Mark)하고, 메모리를 회수(Sweep)한다. 그리고 살아남은 객체를 다른 지역으로 이동시키게 된다. 복제되는 지역이 Available/Unused 지역이면 해당 지역은 이제 Survivor 영역이 되고, Eden 영역은 Available/Unused 지역이 된다.

- Major GC(Full GC)
  - 시스템이 계속 운영되다가 객체가 너무 많아 빠르게 메모리를 회수 할 수 없을 때 -> Major GC(Full GC)가 실행
 - 기존의 다른 GC 알고리즘은 모든 Heap의 영역에서 GC가 수행 -> 처리 시간이 오래 걸림
 - G1 GC는 어느 영역에 가비지가 많은지를 알고 있기 때문에 GC를 수행할 지역을 조합하여 해당 지역에 대해서만 GC를 수행
 - 그리고 이러한 작업은 Concurrent하게 수행되기 때문에 애플리케이션의 지연도 최소화할 수 있다.

<br>
 
- 물론 G1 GC는 다른 GC 방식에 비해 잦게 호출될 것이다. 하지만 작은 규모의 메모리 정리 작업이고 Concurrent하게 수행되기 때문이 지연이 크지 않으며, 가비지가 많은 지역에 대해 정리를 하므로 훨씬 효율적이다.

- G1 GC는 앞의 어떠한 GC 방식보다 처리 속도가 빠르며 큰 메모리 공간에서 멀티 프로레스 기반으로 운영되는 애플리케이션을 위해 고안되었다. 
- 또한 G1 GC는 다른 GC 방식의 처리속도를 능가하기 때문에 Java9부터 기본 가비지 컬렉터(Default Garbage Collector)로 사용되게 되었다.



---
[Java Reference와 GC](https://d2.naver.com/helloworld/329631)  
[Java Garbage Collection](https://d2.naver.com/helloworld/1329)  
[Java 의 GC는 어떻게 동작하나?](https://mirinae312.github.io/develop/2018/06/04/jvm_gc.html)  
[[Java] 다양한 종류의 Garbage Collection(가비지 컬렉션) 알고리즘 (2/2)](https://mangkyu.tistory.com/119)