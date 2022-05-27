# [Tech Interview]

Java Instrumentation 이란 무엇이고 사용했을 때 어떤 장점이 있을까요?

 <br>

---

## Byte Code Instrumentation

Java에서 가장 원초적이고 강력한 프로그래밍 기법입니다.

Byte Code Instrumenation이란 Java의 Byte Code에 대해 직접 수정을 가해서, 
소스 파일의 수정없이 원하는 기능을 부여하는 기법을 말합니다.

BCI를 통해 모니터링 대상이 되는 어플리케이션의 수정없이 성능 측정에 필요한 요소들을 삽입할 수 있습니다. 

Bytecode를 직접 수정할 수 있기 때문에 BCI를 통해서 구현할 수 있는 기능은 그야말로 무궁무진하다고 할 수 있습니다.

### Java Bytecode

Java가 Bytecode라는 일종의 기계어(머신코드)를 사용한다는 것은 익히 알려진 사실입니다.

 

전통적인 기계어가 특정 OS/하드웨어에 의존적인데 반해 Java Bytecode는 JVM(Java Virtual Machine)에만 의존적이라는 중요한 차이가 있습니다. 

따라서 JVM만 동일하다면 어떤 OS/하드웨어에서든 동일한 Bytecode가 구동가능합니다. 

Java가 오늘날 지배적인 언어가 된 것은 바로 OS 중립적인 기계어인 Bytecode 때문입니다.