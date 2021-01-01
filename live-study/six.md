## 6주차 과제

- 자바 상속의 특징
- super 키워드
- 메소드 오버라이딩
- 다이나믹 메소드 디스패치
- 추상 클래스
- final 키워드
- Object 클래스

### 자바 상속의 특징

자바는 C++의 다중 상속과 같은 상속은 지원하지 않는다.

자바에서는 무조건 단일 클래스 상속이며, 여러개를 상속할 경우 컴파일 에러를 발생시킨다.

상속을 통해 계층형 구조를 실현할 수는 있지만, 보통은 인터페이스를 통한 인터페이스 상속을 사용하는 것이 바람직하다.

### super 키워드

super 키워드는 모든 사용자 정의 클래스, 클래스 상속을 받은 자식 클래스에서 사용 가능하다.

super 키워드를 사용하여 부모 클래스의 생성자나, 메서드를 호출 가능하다.

```java
class Order {

    public Order() {
       super();    // Object의 super를 호출한다.
    }
}

class OrderExtension extends Order {

    public OrderExtension() {
        super(); // Order의 생성자를 호출한다.
    }
}
```

### 메소드 오버라이딩

메소드 오버라이딩은 부모 클래스의 인스턴스 메서드를 재정의하는 것이다.

예컨대 아래와 같은 구조로 사용한다.

```java
abstract class AbstractCoupon {

    protected boolean isMinOrderAmount() {
        return false;
    }

    abstract Money calculateDiscountAmount();
}

public class AmountDiscountCoupon extends AbstractCoupon {

  @Override
  public Money calculateDiscountAmount() {
    if(isLessThanMinOrderAmount(totalAmount)) {
      return Money.ZERO;
    }
    return discountAmount;
  }
}
```

### 다이나믹 메소드 디스패치

다이나믹 메소드 디스패치는 오버 라이딩된 메서드들이 있는 경우 어떤 것을 호출할 것인지를 런타임 시에 결정하는 것.

TODO 코드 작성.


### 추상 클래스

Abstract 키워드를 사용하여 정의한다.

추상 클래스는 재사용이나 하나의 기능에 부가 기능을 추가할 때 사용한다.
```java
public abstract class A {

}
```

### final 키워드

final 키워드는 해당 지정자가 작성된 것에 대해 상속이나 변경을 금지한다는 것이다.

```
private final String name = "name"; // 재할당 금지

private final String name; // 생성자에서 초기화해야함.
```

```java
// final 메서드를 만들 경우, 해당 클래스 타입을 상속받는 서브, 하위 클래스에서 오버라이드를 금지한다.
public class A {
  public final void something() {

  }
}

public class B extends A {

}
```

```java
// 클래스의 상속을 금지한다.
public final class StringUtils {

}
```

### Object 클래스

Object 클래스는 모든 클래스의 최상위 클래스이다.

모든 사용자 정의 클래스의 슈퍼 클래스는 Object이다.