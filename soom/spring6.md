# [Tech Interview]

Configuration은 어떻게 Bean을 등록하고 관리할까요? 

(Singleton을 지키는 매커니즘은?)

 </br>

---

## Spring에서 Bean을 등록하는 2가지 방법

1) 클래스마다 @Controller, @Service, @Repository와 같은 어노테이션 붙여주기

2) @Configuration 클래스 내부 메서드에 @Bean 어노테이션 붙여주기


@Configuration과 CGLIB
아래와 같은 @Configuration 클래스를 통해 빈 등록을 할때, stationRepository()는 stationService를 만들 때 한 번, lineService를 만들 때 한 번, 총 2번이 호출 된다.

```java
@Configuration
public class AppConfig {

    @Bean
    public StationService stationService() {
        return new StationServiceImpl(stationRepository());
    }

    @Bean
    public StationRepository stationRepository() {
        return new JdbcStationRepository();
    }

    @Bean
    public LineService lineService() {
        return new LineServiceImpl(stationRepository(), lineRepository());
    }

    // ...
}
```
</br>

그럼 stationService에서 쓰이는 stationRepository()와 lineService에서 쓰이는 stationRepository()는 new로 생성되기 때문에 서로 다른 인스턴스라고 생각할 수 있다.

하지만 주소값을 찍어보면 서로 같은 인스턴스라는 것을 알 수 있다.

이것이 어떻게 가능할까?

</br>

스프링 컨테이너는 기본적으로 빈을 싱글톤으로 관리해준다. 

하지만 위의 구조에서는 new 연산이 2번 불린다. 

이를 싱글톤으로 관리해주기 위해서 스프링은 @Configuration 어노테이션이 붙어 있다면, @Configuration은 클래스의 바이트 코드를 조작하는 라이브러리인 CGLIB를 사용하여 싱글톤을 보장한다. 

@Bean이 등록된 메소드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다. 이 덕분에 싱글톤이 보장되는 것이다.

내부 메서드에 @Bean이 붙어 있다면, @Configuration이 없더라도 빈으로 등록은 되지만, 싱글톤은 보장되지 않는다.