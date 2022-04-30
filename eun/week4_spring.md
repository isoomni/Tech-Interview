## Configuration은 어떻게 Bean을 등록하고 관리할까요? (Singleton을 지키는 매커니즘은?)
#### week4 - 2022-04-30


스프링에서 Bean을 등록할 때 `@Configuration` 어노테이션이 붙은 클래스 내부에 `@Bean` 어노테이션을 붙여 관리하는 방법이 있었다. 이렇게 하면 스프링 컨테이너가 빈을 싱글톤으로 관리하게 된다.

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
<br> 

위의 코드에서 보면, stationService의 stationRepository()와 lineService의 stationRepository()가 서로 다른 인스턴스일 것 같지만 사실 **같은 인스턴스**이다.

### new 로 생성된 두 인스턴스가 같은 이유(Singleton으로 관리하는 방법)
스프링은 `@Configuration` 어노테이션이 붙어 있다면 **바이트코드를 조작**해서 싱글톤으로 빈을 관리한다.

<br> 

`@Configuration` 어노테이션이 붙어 있는 클래스 자체도 빈으로 등록되는데, 이때 우리가 만든 클래스가 등록되는 것이 아니라 스프링에 의해 조작된 클래스가 빈으로 등록된다.

> `@Configuration`을 찍어보면 클래스 명 뒤에 EnhancerBySpringCGLIB가 붙는다.
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

> `CGLIB` : 바이트 코드 조작 라이브러리. 런타임에 동적으로 자바 클래스의 프록시를 생성해주는 기능을 제공.


