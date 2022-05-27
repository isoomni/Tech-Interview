## Spring으로 개발하실 때 어떠한 DI 방식을 사용하시나요? 사용하는 이유는 무엇이 있는 것일까요?
#### week7 - 2022-05-26

의존성 주입 방법에는 세 가지가 있습니다.

1. 생성자 주입(Constructor Injection) : 객체가 생성되는 시점에 의존성 주입
2. 세터 주입(Setter Injection) : 객체의 set Method 호출되는 시점에 의존성 주입
3. 필드 주입(Field Injection) : 객체의 인스턴스 필드에 의존성 주입

그중에서 생성자 주입 방식이 가장 안전하기 때문에 생성자 주입 방식을 사용한다.

[이유]

1. 단일 책임의 원칙(SRP)을 지키기가 쉽다.
   - 한눈에 해당 객체에 대한 의존관계와 복잡도를 알 수 있다.
   - \* 단일 책임의 원칙 : 하나의 객체는 반드시 하나의 동작만의 책임을 갖는다는 원칙 

2. 테스트 코드의 작성이 용이하다.
   - DI컨테이너를 사용하지 않고도 인스턴스화 할 수 있고, 다른 DI 프레임워크로 쉽게 바꿀 수 있다

3. 객체의 불변성을 확보할 수 있다. 
   - 생성자로 주입했기 때문에, 객체의 불변성(final)을 보장할 수 있다

4. 순환 참조 에러를 방지할 수 있다.
   - 생성자 주입에서는 객체가 서로 순환 의존성을 가질 경우 `BeanCurrentlyInCreationException` 이 발생한다

---
참고사이트
[Spring DI(Dependency Injection) & IoC Container - 의존성 주입이란?](https://huisam.tistory.com/entry/springDI)  
[[Spring] 다양한 의존성 주입 방법과 생성자 주입을 사용해야 하는 이유 - (2/2)](https://mangkyu.tistory.com/125)