# [Tech Interview]

Java 접근 제어자에는 무엇이 있는지 설명해주시고 Protect와 Private는 어느 시점에 어떻게 사용될 수 있는지 이야기 해주세요.

</br>

## Java 접근 제어자

+ private
+ default
+ protected
+ public

 private -> default -> protected -> public  순으로 보다 많은 접근을 허용합니다.

 
|접근 제어자|설명|
|-------|------------------------|
|public|	모두가 접근 가능|
|protected|	같은 패키지 내의 클래스만 접근 가능, 상속 받은 경우에도 가능|
|default|	같은 패키지 내의 클래스만 접근 가능|
|private|	같은 클래스일 때만 접근 가능|
 
 </br>

## 접근 제어자를 왜 사용할까?

접근 제어자를 구분해서 사용하지 않고 모두 public으로 해줘도 프로그램은 무리 없이 돌아갈 것입니다.

하지만 접근제어자를 이용하면, 프로그래머의 코딩 실수를 방지할 수 있고 기타 위험 요소를 제거해줄 수 있겠죠?

  </br>

 
## private

접근 제어자가 private이라면, private이 붙은 변수, 메소드는 해당 클래스에서만 접근이 가능합니다.

 
```java
public class User {
	private int userId;
	private String userName;
}
```

```java
public static void main(String[] args) {
	User user = new User();
	System.out.println(user.userId);  // 다른 클래스의 접근이 불가합니다.
}
```

위의 예시는 에러를 반환합니다. private의 접근 제어자를 가진 변수에 접근할 수 없기 때문입니다.

 

  </br>
  

## default

같은 패키지 내에 있는 클래스의 경우에 접근이 가능합니다.

접근 제어자를 별도로 설정하지 않는다면 default 제어자로 설정됩니다.

 

--user/User.java--

```java
package user;  // 동일한 패키지

public class User {
    String userId = "soomni";  
    // userId는 접근제어자를 설정하지 않았기 때문에 default 접근제어자로 설정됩니다.
}
```

--user/PrintUser.java--
```java
package user;  // 동일한 패키지

public class PrintUser {
    public static void main(String[] args) {
        User user = new User();
        System.out.println(user.userId + "@gmail.com");  // User Class의 userId 변수를 사용할 수 있습니다.
    }
}
```

</br>


## protected


동일 패키지의 클래스 또는 해당 클래스를 상속받은 다른 패키지의 클래스도 접근이 가능합니다.

 

--user/User.java--
```java
package user;  // 동일한 패키지

public class User {
    protected String userId = "soomni";  
}
```

```java
package user;  // 동일한 패키지

public class PrintUser extends User { // User를 상속했다.
    public static void main(String[] args) {
        PrintUser printUser = new PrintUser();
        System.out.println(printUser.userId + "@gmail.com");  
        // User를 상속받은 PrintUser Class에서 User Class의 protected 변수에 접근했다.
    }
}
```

User Class의 userId 변수가 protected가 아니라 default 였다면 pringUser.userId 는 컴파일 오류를 발생시킵니다.

 

이게 기본이 되는 정의이지만, 책을 보다보니 몇 가지 기억해야 할 점이 있었습니다.

 

protected의 경우 서로 다른 파일 안에 같은 이름의 패키지가 있을 때,

a 파일의 c 패키지에서 b 파일의 c 패키지의 public, default, protected 멤버에 모두  자유롭게 접근할 수 있습니다.

 

  </br>

## public


마지막으로 public은 어떤 클래스에서도 public 접근제어자가 붙은 변수, 메소드에 접근이 가능합니다.

```java

package user;  // 동일한 패키지

public class User {
    public String userId = "soomni";  
}
```
userId 변수는 어떤 클래스에서 호출하든 접근이 가능하겠죠?

 
</br>

### 마지막으로 접근 시 기억해야 할 점이 있습니다.


- 상속을 받지 않았다면 객체 멤버는 객체를 생성한 후 객체 참조 변수를 이용해서 접근해야 합니다.
 

- 정적 멤버는 클래스명.정적멤버 형식으로 접근하는 것을 권장한다고 합니다.

 

 </br>

정적멤버에 대해서는 다음 포스팅으로 알아보도록 하겠습니다!

