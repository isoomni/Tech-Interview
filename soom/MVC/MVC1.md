## MVC 1, 2 개념에 대해서 설명해 주세요.
---
### 1. MVC

- Model, View, Controller의 줄임말로써, MVC는 사용자와 상호작용하는 S/W를 디자인함에 있어 세가지 요소로 쪼개어 하는 것을 가르킨다.

1) Model- 프로그램의 내부 상태, 즉 프로그램의 정보(데이터)를 말하는 것이다.
2) Controller- 데이터와 비즈니스 로직 간의 상호 작용을 뜻함
3) View- 사용자 인터페이스 요소를 뜻하는데, 유저에게 보여지는 것을 말한다.


### 2. MVC1
![dispatcher servelet](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbRDoUg%2FbtqJScbnSvz%2FtpigVCk3KowkrIx5rWw1p1%2Fimg.png)

- JSP로 구현한 기존 웹 어플리케이션은 모델 1 구조로 웹 브라우저의 요청을 JSP 페이지가 받아서 처리 하는 구조이다.
- JSP 페이지에 비지니스 로직을 처리 하기 위한 코드와 웹 브라우저에 결과를 보여주기 위한 출력 관리 코드가 뒤섞여 있는 구조
- JSP 페이지 안에서 모든 정보를 표현(view)하고 저장(model)하고 처리(control)되므로 재사용이 힘들고, 읽기도 힘들어 가독성이 떨어진다.

#### 정리
- 정의: 모든 클라이언트 요청과 응답을 JSP가 담당하는 구조
- 장점: 단순한 페이지 작성으로 쉽개 구현 가능하다. 중소형 프로젝트에 적합
- 단점: 웹 애플리케이션이 복잡해지면 유지보수 문제가 발생된다.


### 3. MVC2
![dispatcher servelet](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbTYL5S%2FbtqJKwWYzUs%2FckgBdzWVJUVW9zm5VEMEs1%2Fimg.jpg)

- MVC1 구조와 달리 웹 브라우저의 요청을 하나의 서블릿이 받게 됨
- 서블릿은 웹 브라우저의 요청을 알맞게 처리한 후 그 결과를 JSP 페이지로 포워딩

#### 정리
- 정의: 클라이언트의 요청처리와 응답처리, 비지니스 로직 처리하는 부분을 모듈화시킨 구조
- 장점: 처리작업의 분리로 인해 유지보수와 확장이 용이하다.
- 단점: 구조 설계를 위한 시간이 많이 소요되므로 개발 기간이 증가한다.

4. 스프링 MVC 프레임워크

- 스프링이 제공하는 트랜잭션 처리, DI, AOP를 손쉽게 사용
- 스트럿츠2와 같은 프레임워크와 연동이 쉬움
