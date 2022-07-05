## Spring MVC에서 HTTP 요청이 들어왔을 때의 흐름을 설명해 주세요.
---

● 스프링 웹 요청 처리 흐름(DispatcherServlet이 Controller에 전달하는 과정)

![흐름](https://terasolunaorg.github.io/guideline/1.0.1.RELEASE/en/_images/RequestLifecycle.png)

Step1. DispatcherServlet은 web.xml에 정의된 URL 패턴에 맞는 요청을 받고 URL 컨트롤러의 맵핑 작업을 HandlerMapping에 요청



Step2. HandlerMapping은 URL을 기준으로 어떤 컨트롤러를 사용할지 결정, 결과는 HandlerExecution Chain객체에 담아서 리턴하는데, 요청에 해당하는 Interceptor가 있을 경우 함께 줌.



Step3. HandlerAdapter는 컨트롤러의 메소드를 호출하는 역할을 하는데 실행될 Interceptor가 있을 때는 Interceptor의 preHandle() 메소드를 실행한 다음 컨트롤러의 메소드를 호출하여 요청 처리



Step4. 컨트롤러는 요청을 처리 한 뒤 처리한 결과 및 ModelAndView를 DispatcherServlet에 전달.



Step5. DispatcherServlet은 컨트롤러에서 전달받은 View 이름과 매칭되는 실제 View 파일을 찾기 위해 ViewResolver에게 요청.



Step6. ViewResolver는 컨트롤러가 처리한 결과를 보여줄 뷰를 결정, 컨트롤러에서 전달받은 View 이름의 앞뒤로 prefix, suffix 프로퍼티를 추가한 값이 실제 사용할 뷰 파일 경로가 됨. ViewResolver는 맵핑되는 View 객체를 DispatcherServlet에 전달.



Step7. DispatcherServlet은 ViewResolver에 전달받은 View Model을 넘겨서 클라이언트에게 보여줄 화면을 생성.