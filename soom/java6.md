# [Tech Interview]

Java는 Call By Value일까요, Call By Reference 일까요?

https://www.javadude.com/articles/passbyvalue.htm

 </br>

---

## Call By Value
Call by value란, 값을 호출하는 것을 의미합니다. 전달받은 값을 복사하여 처리합니다. 즉 전달받은 값을 변경하여도 원본은 변경되지 않습니다.


## Call By Reference

Call by reference란 참조에 의한 호출을 의미합니다. 전달받은 값을 직접 참조합니다. 즉 전달받은 값을 변경할 경우 원본도 같이 변경이 됩니다.

</br>

---

```

- Pass By Value
    
    값을 전달한다. 값만 전달한다.
    
    복제술처럼 메소드의 매개 변수로 넘길 때에는 원래 값은 놔두고, 전달되는 값이 진짜인 것처럼 보이게 한다.
    
    그래서, 매개 변수를 받은 메소드에서 그 값을 어떻게 지지고 볶던 간에 원래의 값은 변하지 않는다.
    
    - 기본 자료형은 무조건 pass by value로 데이터를 전달한다.


- Pass By Reference

    - 참조 자료형은 Pass By Reference로 데이터를 전달한다.
    
    만약 매개 변수로 받은 참조 자료형 안에 있는 개체를 변경하면, 호출한 참조 자료형 안에 있는 객체는 호출된 메소드에서 변경한 대로 데이터가 바뀐다.
    
    그래서 값이 아니라 객체에 대한 참조가 넘어가는 것을 Pass By Reference라고 한다.
```

Java는 기본적으로 모든 전달 방식이 Call by value입니다.

Java에서는 원본 데이터가 변경되지 않지만, C++에서는 Call by Reference를 통해 원본이 변경된 것을 확인할 수 있습니다.



### 결론

자바는 call by value이다. 메소드에서 작용을 해서 값이 변하는 것처럼 보이더라도, 실제로 넘기는 값은 객체의 실제 주소를 넘기는 것이 아니라 객체를 넘기는 것이다.
