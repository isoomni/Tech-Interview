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

이 중에서 운영 서버에서 절대 사용하면 안 되는 방식이 Serial GC다. Serial GC는 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해서 만든 방식이다. Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어진다.

그럼 각 GC 방식에 대해서 살펴보자.



---
https://d2.naver.com/helloworld/329631
https://d2.naver.com/helloworld/1329
https://mirinae312.github.io/develop/2018/06/04/jvm_gc.html
https://mangkyu.tistory.com/118