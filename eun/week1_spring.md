## Bean이란 무엇인가요?
### week1 - 2022-04-08

### 스프링 빈(bean)이란?
- Spring IoC 컨테이너가 관리하는 자바 객체

> #### 제어의 역전이란?
>
> - 객체의 생성 및 제어권을 사용자가 아닌 <u>**스프링에게 맡기는 것**</u>
> - 지금까지는 사용자가 new연산을 통해 객체를 생성하고 메소드를 호출
>>- 일반적으로 처음에 배우는 자바 프로그램에서는 각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성하고 조작하는 작업(객체를 직접 생성하여 메소드 호출)을 했다. => 모든 작업을 <u>**사용자가 제어**</u>하는 구조  
>>- 예를 들어 A 객체에서 B 객체에 있는 메소드를 사용하고 싶으면, B 객체를 직접 A 객체 내에서 생성하고 메소드를 호출.
> - IoC가 적용된 경우에는 이러한 객체의 생성과 사용자의 <u>**제어권을 스프링에게**</u> 넘긴다.
> - 객체의 생명주기를 컨트롤하는 주체는 스프링이 된다.


- 사용자는 직접 new를 이용해 생성한 객체를 사용하지 않고, 스프링에 의하여 관리당하는 자바 객체를 사용한다. 이 객체를 '빈(bean)'이라 한다.

<br> 

### 스프링 빈(bean)을 스프링 컨테이너에 등록하는 방법

#### 1. 컴포넌트 스캔과 자동 의존관계 설정

- `@Component` 어노테이션이 있으면 스프링 빈으로 자동 등록된다. 
- `@Component`를 포함하는 어노테이션도 스프링 빈으로 자동 등록된다.
  -  `@Controller`, `@Service`, `@Repository` 는 `@Component` 어노테이션을 포함하고 있다. 

- `@RestContoller` 안에는 `@Controller`가 포함되어있고, `@Controller`안에는 `@Component`가 있음을 확인할 수 있다.  
- ![spring](/eun/image/week1_spring1.png)![spring](/eun/image/week1_spring2.png)

- `@Service` 안에는 `@Component`가 있음을 확인할 수 있다.  
- ![spring](/eun/image/week1_spring3.png)

- `@Repository` 안에는 `@Component`가 있음을 확인할 수 있다. 
- ![spring](/eun/image/week1_spring4.png)

<br>

그리고 `@Autowired`를 이용해 연관된 객체 설정해주면 된다.

#### 2 자바 코드로 직접 스프링 빈 등록하기(Configuration)

- `@Configuration`과 `@Bean` 어노테이션을 이용해 스프링 빈을 등록한다. 
- `@Configuration`을 이용하면 스프링 프로젝터에서 Configuration 역할을 하는 Class를 지정할 수 있다.

- configuration 설정 class를 만들고, `@Configuration` 어노테이션을 붙인 다음 Bean으로 등록하고자 하는 Class에 `@Bean` 어노테이션을 붙여준다.

```java
    @Configuration
    public class SpringConfig {

        @Bean
        public MemberService memberService() {
            return new MemberService(memberRepository());
        }

        @Bean
        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
    }
```

---
참고사이트

[스프링 빈(Spring Bean)이란? 개념 정리](http://melonicedlatte.com/2021/07/11/232800.html)  
[[Spring Boot] 스프링 빈(bean)이란? 스프링 빈 등록하는 방법](https://velog.io/@falling_star3/Spring-Boot-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88bean%EA%B3%BC-%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84)