## Bean Scope와 종류에 대해 아는 만큼 설명해주세요.
#### week2 - 2022-04-13

### Bean Scope란?
- 스프링에서는 기본적으로 모든 bean을 singleton으로 생성하여 관리한다.
- 싱글톤이란 애플리케이션 구동 시 JVM 안에서 스프링이 <u>**bean마다 하나의 객체**</u>를 생성하는 것을 의미한다.
- 스프링을 통해서 bean을 제공받으면 언제나 주입받은 bean은 동일한 객체라는 가정하에서 개발을 진행한다.
- 하지만 요구사항에 따라 싱글톤이 아닌 방법으로 bean을 구성해야 하는 경우가 있는데, 이와 같은 경우를 명시적으로 구분하기 위해 스프링에서는 scope라는 키워드를 사용한다.
  
### Bean Scope 종류

1. singleton
- 스프링에서 bean 등록시 따로 설정을 하지 않았다면 싱글턴으로 등록된다. 
- 어플리케이션에 오직 하나만 생성되는 일반적인 scope
- `@Component` 혹은 `@Configuration` + `@bean`의 조합으로 등록된 bean들의 기본 scope

- 빈 스코프를 지정하는 방법
  - [XML을 이용한 방법]
    ```xml
    <bean id="..." class="..." scope="singleton"></bean>
    ```
  - [어노테이션을 이용한 방법]  
    ```java
    @Scope("singletone")
    ```

1. prototype
- prototype scope는 컨테이너에게 빈을 **요청할 때마다** 매번 **새로운 객체**를 생성해줍니다.
- 프로토타입을 받은 클라이언트가 객체를 관리하여 준다.

- 빈 스코프를 지정하는 방법
  - [XML을 이용한 방법]
    ```xml
    <bean id="..." class="..." scope="singlprototypeeton"></bean>
    ```
  - [어노테이션을 이용한 방법]  
    ```java
    @Scope("prototype")
    ```
3. web scope
- 웹환경에서만 동작하는 scope
- 종류로는 request, session, application, websocket이 있다.

- 3-1. request
  - request가 생성되면 bean이 생성되고 종료된다.
  - api가 첫번째 호출되었을때와 두번째 호출되었을때의 bean은 다르다.

    ```java
        @Component
        @Scope(value = "request")
        public class ConvertUtil {
        }
    ```
    <br>
  - Spring 4.3 부터는 @RequestScope 어노테이션을 지원한다. 
    ```java
        @Component
        @RequestScope
        public class ConvertUtil {
        }
    ```

- 3-2 session
    - session과 동일한 생명 주기를 가지는 scope

- 3-3. application
    - 웹의 서블릿 컨텍스와 동일한 생명주기를 가지는 scope

- 3-4. websocket
    - 웹소켓과 동일한 생명주기를 가지는 scope

---
참고사이트  
[[Spring] Spring Bean의 개념과 Bean Scope 종류](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)  
[[SPRING] SPRING BEAN SCOPE 종류](https://yhmane.tistory.com/221)  
[[Spring] Bean Scope와 Bean Life Cycle](https://ooeunz.tistory.com/107)