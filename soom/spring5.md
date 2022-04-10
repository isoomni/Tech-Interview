# [Tech Interview]
</br>

## Bean이란 무엇인가요?

쉽게 말하면 자바 객체입니다.

Spring IoC 컨테이너가 관리하는 자바 객체를 빈(Bean)이라는 용어로 부릅니다.

>스프링 빈과 자바 일반 객체와의 차이점은 없습니다. 다만 스프링 IoC 컨테이너에서 만들어지는 객체를 스프링 빈이라고 부를 뿐이죠.

>빈은 스프링 Ioc 컨테이너에 의해서 인스턴스화되어 조립되거나 관리되는 개체를 말합니다.

이 같은 점을 제외하고는 빈은 수많은 객체들 중의 하나일 뿐입니다.

빈과 빈 사이의 의존성은 컨테이너가 사용하는 메타 데이터 화경설정에 반영됩니다.

또, 우리가 new 연산자로 어떤 객체를 생성했을 때 그 객체는 Bean이 아닙니다.
ApplicationContext.getBean()으로 얻어질 수 있는 객체가 Bean입니다.

즉, Spring에서의 빈은 ApplicationContext가 알고있는 객체, 즉 ApplicationContext가 만들어서 그 안에 담고있는 객체를 의미합니다.

---
</br>


## 어떻게 Spring IoC 컨테이너에 빈을 등록할까?

크게 두 가지 방법이 있습니다.
1. Component Scanning
2. Bean 설정파일에 직접 Bean을 등록

</br>

### 1. Component Scanning

`@ComponentScan` 어노테이션과 `@Component` 어노테이션을 사용해서 빈을 등록하도록 하는 방법입니다.

간단히 말하면 `@ComponentScan` 어노테이션은 어느 지점부터 컴포넌트를 찾으라고 알려주는 역할을 하고,

 `@Component`는 객체를 실제로 찾아서 빈으로 등록할 클래스를 의미합니다.

Spring IoC 컨테이너가 IoC 컨테이너를 만들고 그 안에 빈을 등록할때 사용하는 인터페이스들을 `라이프 사이클 콜백`이라고 부릅니다.

`라이프 사이클 콜백` 중에는 `@Component` 어노테이션을 찾아서 이 어노테이션이 붙어있는 모든 클래스의 인스턴스를 생성해 빈으로 등록하는 작업을 수행하는 어노테이션 프로세서가 등록되어 있습니다.

 
Spring Boot 프로젝트에서 `@ComponentScan` 어노테이션이 붙어있는 클래스가 이에 해당합니다.


</br>

### 2. Bean 설정파일에 직접 Bean을 등록하는 방법

위와 같이`@Component` 어노테이션을 사용하는 방법 말고도 빈 설정파일에 직접 빈으로 등록할 수 있습니다.

빈 설정파일은 **XML**과 **자바 설정파일**로 작성할 수 있는데 최근 추세는 **자바 설정파일**을 좀 더 많이 사용합니다.

자바 설정파일은 자바 클래스를 생성해서 작성할 수 있으며 일반적으로 **xxxxConfiguration**와 같이 명명합니다.

그리고 클래스에 `@Configuration `애노테이션을 붙입니다.

그 안에 `@Bean` 애노테이션을 사용해 직접 빈을 정의합니다.

```java
@Configuration
public class SampleConfiguration {
    @Bean
    public SampleController sampleController() {
        return new SampleController;
    }
}
```
sampleController()에서 리턴되는 객체가 IoC 컨테이너 안에 빈으로 등록됩니다.

 
</br>

물론 이렇게 빈을 직접 정의해서 등록하면 `@Component` 애노테이션을 붙이지 않아도 됩니다.

`@Configuration` 애노테이션을 보면 이 애노테이션도 `@Component`를 사용하기 때문에 `@ComponentScan`의 스캔 대상이 되고 그에 따라 빈 설정파일이 읽힐때 그 안에 정의한 빈들이 IoC 컨테이너에 등록되는 것입니다.
