## 5주차

- 클래스를 정의하는 방법
- 객체 만드는 방법(new 키워드 이해하기)
- 메소드 정의하는 방법
- 생성자 정의하는 방법
- this 키워드 이해하기

### 클래스를 정의하는 방법

클래스는 필드, 생성자, 메서드를 정의할 수 있습니다.
```java
class Order {
    private String orderId;
    private int count;
    private Product product;

    public Order(String orderId, int count, Product product) {
        this.orderId = orderId;
        this.count = count;
        this.product = product;
    }

    public void place() {
        // something
    }
}
```

- 필드: 객체의 속성을 정의하는데 사용된다.
- 생성자: 객체를 생성하는 용도이다.
- 메서드: 객체의 행동을 정의한 것이다.

이외에도 접근제어자(public, private)가 필요하며, 클래스명은 무조건 대문자로 해야한다.
또한 상속은 하나만 가능하고, implements는 여러개로 구현 가능하다.

### 객체 만드는 방법(new 키워드 이해하기)

```java
// Type name = new Type(Parameter);
Product product = new Product("stone1", 2, 2000);
```

타입, 인스턴스명, new Type으로 정의된다.

Type name만 선언하고, 인스턴스화 하지 않고 사용한다면 컴파일 에러가 발생합니다.

객체를 만들때, 생성자의 아무것도 정의하지 않은 채로 생성하면, 기본 생성자가 컴파일러 상에서 기본 생성자가 제공된다. 이때 생성자는 Object의 생성자이다.

### 메소드 정의하는 방법

```java
public void place(OrderValidator validator) {

}
```

메서드의 이름과, 파라미터 타입을 통틀어서 메서드 시그니처라고 합니다.

아래와 같이 메서드의 시그니처를 다르게 하고 같은 이름의 메서드를 여러개 만들 수 있습니다. 이를 메서드 오버로딩이라 합니다.

```java
public static Order place() {

}

public static Order place(String name) {
    
}

public static Order place(String name, Integer price) {
    
}
```

### 생성자 정의하는 방법

생성자는 여러개 생성 가능합니다.
```java
public Order(String name, Integer price) {

}
```

### this 키워드 이해하기

this는 객체의 인스턴스 자기 자신을 의미합니다. this를 사용하여 객체의 메서드, 생성자 내에서 멤버 변수를 참조할 수 있습니다.

this()를 통해 생성자 내에서 초기화를 선택적으로 정의할 수 있습니다.

```java
public class Order {
    private int price;
    private String name;

    public Order(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public Order() {
        this("", 1);
    }

    public Order(String name) {
        this(name, 1);
    }
}
```