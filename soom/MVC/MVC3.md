
## 1. Dispatcher-Servlet(디스패처 서블릿)의 개념

### [Dispatcher Servlet 이란?]
디스패처 서블릿의 dispatch는 "보내다"라는 뜻을 가지고 있습니다. 

그리고 이러한 단어를 포함하는 디스패처 서블릿은 HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 

적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)라고 정의할 수 있습니다.

이것을 보다 자세히 설명하자면, 클라이언트로부터 어떠한 요청이 오면 

Tomcat(톰캣)과 같은 서블릿 컨테이너가 요청을 받게 됩니다. 

그리고 이 모든 요청을 프론트 컨트롤러인 디스패처 서블릿이 가장 먼저 받게 됩니다. 

그러면 디스패처 서블릿은 공통적인 작업을 먼저 처리한 후에 해당 요청을 처리해야 하는 컨트롤러를 찾아서 작업을 위임합니다.

여기서 Front Controller(프론트 컨트롤러)라는 용어가 사용되는데, 

Front Controller는 주로 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러로써, MVC 구조에서 함께 사용되는 디자인 패턴입니다.



### [Dispatcher Servlet의 장점]

Spring MVC는 DispatcherServlet이 등장함에 따라 web.xml의 역할을 상당히 축소시켜주었습니다. 

과거에는 모든 서블릿을 URL 매핑을 위해 web.xml에 모두 등록해주어야 했지만, 

dispatcher-servlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주고

공통 작업을 처리면서 상당히 편리하게 이용할 수 있게 되었습니다. 

우리는 컨트롤러를 구현해두기만 하면 디스패처 서블릿가 알아서 적합한 컨트롤러로 위임을 해주는 구조가 되었습니다.



### [정적 자원의 처리]

Dispatcher Servlet이 요청을 Controller로 넘겨주는 방식은 효율적으로 보입니다. 

하지만 Dispatcher Servlet이 모든 요청을 처리하다보니 이미지나 HTML/CSS/JavaScript 등과 같은 정적 파일에 대한 요청마저 

모두 가로채는 까닭에 정적자원(Static Resources)을 불러오지 못하는 상황도 발생하곤 했습니다.

이러한 문제를 해결하기 위해 개발자들은 2가지 방법을 고안했습니다.


1. 정적 자원에 대한 요청과 애플리케이션에 대한 요청을 분리
이에 대한 해결책은 두가지가 있는데 첫번째는 클라이언트의 요청을 2가지로 분리하여 구분하는 것입니다.

- /apps 의 URL로 접근하면 Dispatcher Servlet이 담당한다.
- /resources 의 URL로 접근하면 Dispatcher Servlet이 컨트롤할 수 없으므로 담당하지 않는다.

    
이러한 방식은 괜찮지만 상당히 코드가 지저분해지며, 모든 요청에 대해서 저런 URL을 붙여주어야 하므로 직관적인 설계가 될 수 없습니다. 그래서 이러한 방법의 한계를 느끼고 다음의 방법으로 처리를 하게 되었습니다.

2. 애플리케이션에 대한 요청을 탐색하고 없으면 정적 자원에 대한 요청으로 처리



## 2. Dispatcher Servlet의 동작과정

### [Dispatcher-Servlet(디스패처 서블릿)의 동작 방식

앞서 설명한대로 디스패처 서블릿은 적합한 컨트롤러와 메소드를 찾아 요청을 위임해야 합니다. Dispatcher Servlet의 처리 과정을 살펴보면 다음과 같습니다.

![dispatcher servelet](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FY5nZW%2FbtrsHGTIP02%2FO3ff3AhmLzkCkknoImVTK0%2Fimg.png)


1. 클라이언트의 요청을 디스패처 서블릿이 받음
2. 요청 정보를 통해 요청을 위임할 컨트롤러를 찾음
3. 요청을 컨트롤러로 위임할 핸들러 어댑터를 찾아서 전달함
4. 핸들러 어댑터가 컨트롤러로 요청을 위임함
5. 비지니스 로직을 처리함
6. 컨트롤러가 반환값을 반환함
7. HandlerAdapter가 반환값을 처리함
8. 서버의 응답을 클라이언트로 반환함