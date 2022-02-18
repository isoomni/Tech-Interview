# [Tech Interview]

IoC, DI는 무엇이고 어떠한 장점이 있을까요?

</br>

---

## IOC 란 무엇인가?

IOC 는 SPRING 용어가 아닙니다.SPRING 이전에도 존재했던 용어죠.

IOC는 Inversion of Control 의 약자입니다.

 </br>

단어를 좀 뜯어볼까요?

Inversion 역전

Control 제어

 </br>

즉, IOC는 제어의 역전입니다.

 </br>

### 그럼 뭘 제어하고 뭘 역전했다는 것일까요?

 

이전 포스팅에서 SPRING 을 EJB 와 비교해서 설명할 때 제어에 대해서는 많이 이야기를 했습니다.
</br>
 

원래는 제어는 개발자가 구현해야 했습니다.

 </br>

하지만 EJB와 SPRING 모두 편리한 개발, 즉 개발자가 복잡하고 실수하기 쉬운 로우레벨의 기술에 많이 신경쓰지 않고도, 애플리케이션의 핵심인 사용자의 요구사항, 즉 비지니스 로직을 빠르고 효과적으로 구현하게 하는 것을 목표로 했습니다.

 

둘 다 시스템이 개발자 대신 제어를 담당해 줄 수 있도록 시도한 것이죠.

 </br>

그러나 EJB는 메서드나 객체의 호출 작업 등에 대한 제어를 코드에 침투적으로 반영했고, SPRING은 제어와 비지니스 로직을 분리할 수 있게 했다는 차이가 있었습니다.

 </br>

제어의 역전은 이 개념을 통해 이해해 볼 수 있습니다.

원래는 개발자가 직접 객체를 생성하여 제어를 관리해주어야 했습니다.

 </br>

다음을 보면,

```java
public class A {

    private B b;

    public A()
        b = new B();
    }
}
```

"A객체는 B객체에게 의존하고 있어" 라고 개발자가 클래스를 통해 직접 표현해주고 있죠

제어를 개발자가 관리하고 있습니다.

</br>
 

하지만 SPRING에서는 

```java
public class A {

    @Autowired
    private B b;

}
```

@Autowired 를 통해 객체를 주입받았습니다.

 </br>

B라는 객체가 스프링 컨테이너에게 관리되고 있는 Bean이라면 @Autowired 를 통해 객체를 주입받을수 있게 됩니다.

 

이것은 스프링 컨테이너에서 직접(제어) 객체를 생성하여 해당 객체에 주입 시켜준 것입니다.

 </br>

이것이 바로 제어가 역전된 경우입니다.

개발자가 해야 할 제어를 스프링 컨테이너에서 대신 해주고 있으니까요.

이것이 IoC 라는 개념입니다. 

 </br>

## 왜 IoC 를 사용했을까?

### 제어의 역전의 장점

+ 객체 간 결합도를 낮춘다
+ 유연한 코드 작성 가능
+ 가독성 증진
+ 코드 중복 방지
+ 유지 보수 용이

기존에는 객체를 클래스 내부에서 생성하고 사용했지만 IoC를 적용하면 미리 생성해놓은 객체를 주입받아 사용하기만 하면 됩니다.

 
</br>
 

## DI (Dependency Injection)

이제 '객체를 주입받는 것'에 대해 알아볼까요?

'객체를 주입받는 것'이 바로 DI 가 의미하는 바 입니다.

직역하면 '의존성 주입'이죠

 </br>

#### 주입이 객체를 외부에서 넣어준다는 개념이라는 것은 알겠는데 왜 의존성 일까요?

 </br>

### 우선 의존도가 무엇인지 잠시 살펴볼까요?

 
A 클래스 내부에서 B, C 클래스의 메소드를 활용한다고 해봅시다.

유지 보수 과정에서 B, C 클래스의 응답형식이 변경된다면 A 클래스도 변경되어야 합니다.

A 클래스의 로직이 변경되어 B, C 클래스로 보내는 객체가 변경된다면 기존과 같게 변경하는 로직을 추가해야 합니다.

이렇게 B, C 클래스에 의존도가 높은 코드는 결합도가 높은 코드라고 이야기합니다.

A와 B,C는 서로에 대해 의존성이 있다고 하죠.

 </br>

DI는 이런 의존성을 외부에서 주입한다고 합니다.여기서 외부는 스프링 컨테이너를 의미합니다.


그림처럼 A는 B, C라는 메소드가 필요할 떄 스프링 컨테이너로부터 B, C를 주입받습니다.

필요한 의존성을 주입받게 되는 것이죠.

이런 경우 A는 로직이 허용하는 한 B,C를 마음대로 갈아 끼울 수 있습니다.

의존도와 결합도가 낮아집니다.

 
</br>
 

## 그럼 DI 를 하는 방식에는 어떤 것이 있을까요?

+ Field Injection(필드 주입)

+ Setter Injection(수정자 주입)

+ Constructor Injection(생성자 주입) (*추천)

</br>
 

### 1. Field Injection(필드 주입)
 

Field Injection은 의존성을 주입하고 싶은 필드에 @Autowired 어노테이션을 붙여주면 의존성이 주입됩니다.
```java
@RestController
public class PostController {

    @Autowired
    private PostService postService;
    
}
```

1. 주입받으려는 빈의 생성자를 호출하여 빈을 찾거나 빈 팩토리에 등록

2. 생성자 인자에 사용하는 빈을 찾거나 만듦

3. 필드에 주입

 
</br>
 

### 2. Setter Injection(수정자 주입)
 

setter메서드에 @Autowired 어노테이션을 붙여 의존성을 주입하는 방식입니다.

 
```java
@RestController
public class PostController {

    private PostService postService;
    
    @Autowired
    public void setPostService(PostService postService){
    	this.postService = postService;
    }
    
}
```
1. 주입받으려는 빈의 생성자를 호출하여 빈을 찾거나 빈 팩토리에 등록

2. 생성자 인자에 사용하는 빈을 찾거나 만듦

3. 주입하려는 빈 객체의 수정자를 호출하여 주입

 

위의 두 방식은 런타임에서 의존성을 주입하기 때문에 의존성을 주입하지 않아도 객체가 생성될 수 있다.

 
</br>
 

### 3. Constructor Injection(생성자 주입) (*추천)
 

생성자를 사용하여 의존성을 주입하는 방식이다.

 
```java
@RestController
public class PostController {

    private final PostService postService;
    
    public PostController(PostService postService){
    	this.postService = postService;
    }
    
}
```

</br>

#### Constructor based Injection은

1. 생성자의 인자에 사용되는 빈을 찾거나 빈 팩토리에서 만든다.

2. 찾은 인자 빈으로 주입하려는 생성자를 호출한다. 

 
</br>
 

## Constructor Injection(생성자 주입)을 사용해야 하는 이유

+ NullPointerException의 발생을 막는다.

  + 기존 Setter Injection의 단점을 보완한다.

  + 객체가 생성되는 시점에 빈을 주입한다. 생성시에 의존성 없이는 생성 불가능하기 때문에 일부러 null을 주입하지 않는 이상 발생하지 않는다.

+ 불변성을 활용할 수 있다.

  + final로 선언 가능하다.

+ SRP(단일 책임 원칙)을 지킬 수 있도록 유도한다.

  + 사용하기가 약간 불편하고 생성자의 코드가 길어짐을 느낄 수밖에 없게 되어 있다. 따라서 코드가 길어진다면 단일 책임 원칙을 다시 한번 생각하게 해 준다.

