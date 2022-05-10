## Configuration은 어떻게 Bean을 등록하고 관리할까요? (Singleton을 지키는 매커니즘은?)
## Bean Lite Mode는 무엇인가요?
#### week4 - 2022-04-30


스프링에서 Bean을 등록할 때 `@Configuration` 어노테이션이 붙은 클래스 내부에 `@Bean` 어노테이션을 붙여 관리하는 방법이 있었다. 이렇게 하면 스프링 컨테이너가 빈을 싱글톤으로 관리하게 된다.

```java
    @Configuration
    public class AppConfig {

        @Bean
        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }

        @Bean
        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }

        @Bean
        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
    }

    // ...
}
```
<br> 

위의 코드에서 보면, memberService의 memberRepository()와 orderService의 memberRepository()가 서로 다른 인스턴스일 것 같지만 사실 **같은 인스턴스**이다.

### new 로 생성된 두 인스턴스가 같은 이유(Singleton으로 관리하는 방법)
스프링은 `@Configuration` 어노테이션이 붙어 있다면 **바이트코드를 조작**해서 싱글톤으로 빈을 관리한다.

<br> 

`@Configuration` 어노테이션이 붙어 있는 클래스 자체도 빈으로 등록되는데, 이때 우리가 만든 클래스가 등록되는 것이 아니라 스프링에 의해 조작된 클래스가 빈으로 등록된다.

> `@Configuration`을 찍어보면 클래스 명 뒤에 EnhancerBySpringCGLIB가 붙는다.
> CGLIB는 프록시 객체의 일종으로 AppConfig가 빈으로 등록될 때, AppConfig 대신 AppConfig를 상속 받은 AppConfig$CGLIB 형태로 프록시 객체가 등록된다.

> `@Configuration` 대신 `@Component` 등의 다른 어노테이션을 붙여주면 클래스명만 찍힌다.

```java
// @Configuration
class wooteco.subway.AppConfig$$EnhancerBySpringCGLIB$$58ae7df8
```

```java
// @Component
class wooteco.subway.AppConfig
```

<br> 

`@Configuration` 어노테이션이 붙은 클래스는 `CBLIB`을 이용하여 우리가 만든 클래스를 상속받아 임의의 클래스를 만들어주고, 그 클래스가 빈으로 등록되는 것이다. 이 조작된 빈이 싱글톤을 보장해준다.

조작된 클래스는 `@Bean` 어노테이션이 붙어있는 메서드가 실행될 때, 스프링 컨테이너에 해당 빈이 존재하면 그 빈을 찾아서 반환해주고, 없다면 새로 생성해서 빈으로 등록해주고 반환해준다.

내부 메서드에 `@Bean`이 붙어 있다면, `@Configuration`이 없더라도 빈으로 등록은 되지만, **싱글톤은 보장되지 않는다.**



> `CGLIB`(**B**yte **C**ode **G**eneration **Lib**rary) : 클래스의 바이트 코드를 조작하는 라이브러리. 런타임에 동적으로 자바 클래스의 프록시를 생성해주는 기능을 제공.

---

## Bean Lite Mode

- Bean Lite Mode는 CGLIB를 이용하여 바이트 코드 조작을 하지 않는 방식 => 즉, 스프링 빈의 싱글톤을 보장하지 않는다.

- Bean Lite Mode로 설정하는 방법
  - `@Configuration` -> `@Component`로 변경
  - `objectMapperLiteBean()` 메소드를 `lite mode`로 작동하여 매번 다른 객체를 반환


```java
    @Component
    public class AppConfig {

        @Bean
        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }

        @Bean
        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }

        @Bean
        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
    }

```


---
참고사이트  
[[Spring] @Configuration을 통한 빈 등록 시 싱글톤 관리](https://velog.io/@max9106/Spring-Configuration%EC%9D%84-%ED%86%B5%ED%95%9C-%EB%B9%88-%EB%93%B1%EB%A1%9D-%EC%8B%9C-%EC%8B%B1%EA%B8%80%ED%86%A4-%EA%B4%80%EB%A6%AC)  
[[Spring] Spring Bean 총정리](https://steady-coding.tistory.com/594)