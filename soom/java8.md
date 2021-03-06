# [Tech Interview]

Java Reflection이란 무엇이고, 어떨 때 사용되는 것인가요?

 <br>

---

## Java Reflection

리플렉션은 구체적인 클래스 타입을 알지 못하더라도 그 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API입니다.

💡 **컴파일 시간이 아닌 실행 시간에 동적으로 특정 클래스의 정보를 추출할 수 있는 프로그래밍 기법입니다.**


### 리플렉션은 언제 사용될까?
+ 동적으로 클래스를 사용해야할 때 필요합니다.

+ 다시 말해 작성 시점에는 어떠한 클래스를 사용해야 할지 모르지만, 런타임 시점에서 클래스를 가져와서 실행해야하는 경우 필요합니다.

+ 대표적으로는 Spring 프레임워크의 **어노테이션** 같은 기능들이 리플렉션을 이용하여 프로그램 실행 도중 동적으로 클래스의 정보를 가져와서 사용합니다.


