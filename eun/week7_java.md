## Java Instrumentation이란 무엇이고 사용했을 때 어떤 장점이 있을까요?
#### 2022-05-27

### BCI(Byte Code Instrumentation)
- BCI란 런타임이나 로드 시에 클래스의 바이트 코드에 변경을 가하는 기법
- 소스파일의 수정 없이 원하는 기능을 부여할 수 있고 필요한 정보를 추적할 수 있다.
- Spring AOP도 BCI를 사용하고 있다.

### AOP(Aspect-Oriented Programming, 관점 지향 프로그래밍)
- OOP로 독립적으로 분리하기 어려운 부가 기능을 모듈화하는 방식
- 트랜잭션 관리와 같은 부분이 바로 부가 기능 모듈이며, 이를 Aspect라고 한다.
- 핵심 비즈니스 로직을 담고 있지는 않지만 어플리케이션에 부가됨으로써 의미를 갖는 특별한 모듈
- AOP는 핵심 비즈니스 로직과 부가 기능 Aspect를 분리하는 등 OOP를 보완하는 역할

---
참고사이트  
[5월 우아한 Tech 세미나 후기](https://techblog.woowahan.com/2627/)  
[AOP 입문자를 위한 개념 이해하기](https://tecoble.techcourse.co.kr/post/2021-06-25-aop-transaction/)