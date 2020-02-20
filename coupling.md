## Loose Coupling VS Tight Coupling?

느슨한 결합과 강한 결합의 차이가 뭘까?

JPA를 통해 프로젝트를 진행하며 결합도, 응집도에 관해 많은 고민을 했다.

아래의 구조는 일대일 관계를 아주 간단하게 표현한 구조이다.
```java
public class A{
    B b;
}

public class B{
    A a;
}
```

아래는 일대다 다대일의 구조를 아주 간단하게 표현했다.
```java
public class A{
    List<B> bList;
}

public class B{
    A a;
}
```

이런 구조는 사실 JPA를 통한 객체지향적 개발을 할때 도메인과 요구사항을 기준으로 결합도를 낮춰야 한다.

이 부분에 대해서 너무 이해가 안됬고, 결합도를 끊으면서 응집도를 높이는 구조를 아래와 같이 표현해봤다.(여기서 id값은 pk값이라고 가정한다.)
```java
public class A{
    Long bId;
}

public class B{
    Long aId;
}
```

위의 구조로 데이터를 가져온다는 것이 이해가 되는가? 이해가 안된다면 아직 결합도에 관해 완벽히 이해하지 못한
것이다.

```java
public class A{
    Long bId;
    
    public void addB(B b) {
        b.add();
    }
}
```

위와 같은 구조로 id값을 활용해서 데이터베이스에서 가져온 후, B를 사용해 CRUD를 진행하면 되는 것이다.

이 부분이 사실 JPA,객체지향,데이터베이스 모두 알아야 하는 부분이다. 단순히 어느 한 부분의 개념만
알고 있다면 어떻게 JPA를 통해 객체지향적 개발을 하는지 알 수 없다.