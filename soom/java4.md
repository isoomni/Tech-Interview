# [Tech Interview]

JVM은 어떤 방식으로 코드를 해석하고 실행시키는지 흐름에 맞게 설명해 주세요. (Java 실행 흐름)

 </br>


## JVM?
java3.md에서 JVM에 대해 설명했지만, 짧게 다시 한 번 짚어보겠습니다.
JVM은 우리가 작성한 JAVA 프로그램이 수행되는 프로세스를 의미합니다.

다시 말해서, JAVA 라는 명령어를 통해서 애플리케이션이 수행되면, JVM 위에서 애플리케이션이 동작합니다.

이 JVM에서 여러분들이 작성한 프로그램을 찾고 실행하는 일련의 작업이 진행됩니다.

자바는 메모리 관리를 개발자가 해줄 필요가 없는 언어입니다.

메모리 관리를 JVM이 해주기 때문입니다.

이때 JVM 내에서 메모리 관리를 해주는 것을 바로 "가비지 컬렉터"라고 부릅니다. Garbage는 "쓰레기"를 의미하므로, 사용하고 남아있는 전혀 필요 없는 객체들이 여기에 속합니다.

물론 아무리 가비지 컬렉터가 쓰레기를 알아서 청소한다고 하더라도, 메모리를 효율적으로 사용하도록 개발하는 것은 중요합니다.

## JAVA 실행 흐름
JVM은 어떻게 JAVA 코드를 해석하고 실행할까요?

![java](/soom/img/jvm.jpg)

1. 프로그램이 시작되면, JVM은 프로그램에 필요한 메모리를 할당받습니다.
2. 자바 컴파일러는 자바 코드를 바이트 코드로 변환합니다.
3. Class Loader를 통해, `[.class]파일`을 JVM에 로드합니다.
4. 로딩된 `[.class]파일` Execution Engine을 통해 해석됩니다.
5. 해석된 바이트코드는 Runtime Data Area에서 실질적으로 수행됩니다.
6. 이러한 환경 속에서 JVM은 필요에 따라 Synchronized Thread, Garbage Collector를 수행합니다.

---
각 영역 별 세부 설명은 java3.md에 자세히 해두었습니다.
[JVM 메모리 구조](https://github.com/isoomni/Tech-Interview/blob/main/soom/java3.md)  