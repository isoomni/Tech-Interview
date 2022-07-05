## Front Controller Pattern은 무엇인가요?
---
기존의 패턴을 설명하자면 아래 그림과 같다


각 클라이언트들은 Controller A, B, C에 대해 각각 호출한다.

공통 코드들은 별도로 처리되어 있지 않고 각 Controller에 포함되어 있다.

 

하지만 프론트 컨트롤러 패턴을 도입한다면?


각 클라이언트들은 Front Controller에 요청을 보내고,

Front Controller은 각 요청에 맞는 컨트롤러를 찾아서 호출시킨다.

공통 코드에 대해서는 Front Controller에서 처리하고, 서로 다른 코드들만 각 Controller에서 처리할 수 있도록 한다.

 

장점을 정리해보자면,

공통 코드 처리가 가능 (★중요)
Front Controller 외 다른 Controller에서 Servlet 사용하지 않아도 됨