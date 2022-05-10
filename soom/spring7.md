# [Tech Interview]

Bean Lite Mode는 무엇인가요?

 </br>

---

## Bean Lite Mode
Bean Lite Mode는 CGLIB를 이용하여 바이트 코드 조작을 하지 않는 방식을 의미한다. 즉, 스프링 빈의 싱글톤을 보장하지 않는다.

 
```
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

Bean Lite Mode로 설정하려면 @Configuration이 아닌 @Component로 변경하면 된다. 이렇게 하면 objectMapperLiteBean() 메소드를 lite mode로 작동하여 매번 다른 객체를 반환해 줄 수 있다.

참고로 ApplicationContext를 사용해서 설정 파일 가지고 빈을 수동 등록한다면, @Component가 없어도 Bean Lite Mode가 동작한다.