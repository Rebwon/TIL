## VO를 써야하는 이유?

도메인 주도 설계에서 VO(Value Object)를 사용해야 하는 이유가 무엇일까?

애플리케이션을 구성하는 객체들을 Reference Object와 Value Object로 나눌 수 있다.

Reference Object는 고객, 주문, 예매와 같이 실 세계의 추적 가능한 개념을 표현한다.

반면에 Value Object의 경우 날짜, 금액과 같이 보여지는 값이다. 보여지는 값이고 언제든지 변할 수 있기 때문에
객체 추적 가능성에 대해 관심을 두지 않아도 된다.

Reference Object는 유일하기 때문에 동일성 확인 시에 ==를 사용하지만 Value Object의 경우 equals()를 사용하여 속성 값의
동등성을 비교해야 한다.

근본적으로 Reference Object와 Value Object를 나누는 이유가 무엇일까?

이에 대한 해답은 Reference Object 대신 Value Object를 사용함으로써 별칭 문제를 피할 수 있기 때문이다.

메소드 인자에 final을 사용한다고 해도 별칭 문제를 막을 수 없다. java의 final은 C++의 const와 달리 단지 메소드 내부에서 다른 객체를 참조하지 않도록 막아 주는 역할만을 할 뿐이다. 
객체가 final로 전달되더라도 전달된 객체 자체의 상태를 바꾸는 것이 가능하다는 사실에 주의하자.

final 키워드가 붙은 객체는 재할당만 불가능하지, 객체의 상태는 충분히 바꿀 수 있다.

아래의 테스트를 실행하면 테스트가 성공한다.
```java
@Test
public void final도_값변경이_가능하다() {
    // given
    final Map<String, Boolean> collection = new HashMap<>();

    // when
    collection.put("1", true);
    collection.put("2", true);
    collection.put("3", true);
    collection.put("4", true);

    // then
    assertThat(collection.size()).isEqualTo(4);
}
```

따라서, 좋은 객체 지향 습관을 갖고 싶다면 전달된 객체의 상태를 바꾸는 메서드는 작성하지 않는 것이 바람직하다.

별칭 문제를 해결하기 위한 가장 좋은 방법은 불변 객체를 생성하는 것이다.

**불변성(Immutable)**

불변 클래스는 다음과 같은 규칙을 따른다.

- 객체를 변경하는 메서드(setter)를 제공하지 않는다.
- 재정의할 수 있는 메서드를 제공하지 않는다.
- 모든 필드를 final로 만든다.
- 모든 필드를 private으로 만든다.
- 가변(mutable) 객체를 참조하는 필드는 배타적으로 접근해야 한다.

객체를 불변으로 만들면, 별칭 문제를 피할 수 있다. 객체의 상태를 바꿀 수 없으므로 새로운 상태로 변경해야 할 경우 새로운 불변 객체를 만들어
기존의 불변 객체를 대체 시켜야 한다. 객체가 불변이라면 객체를 어디에 어떤 방식으로 노출 시키더라도 예상하지 못한 사이드 이펙트로 인해 놀랄 일은 없어질 것이다.

여담이지만 자바 8 이전의 경우, 날짜 클래스와 관련하여 Date, Calendar 클래스를 사용하였다. 하지만 Date와 Calendar 클래스는 가변 객체로 설계가 되어서
많은 개발자들이 사이드 이펙트를 방어 복사 기법이나 여타 다른 라이브러리(Joda-Time)과 같은 다른 방식으로 해결하곤 했다.

Value Object를 통해 도메인을 설계할 때, 단순히 값이나 날짜 타입은 반드시 불변 객체를 사용하는 것을 고려하는게 좋을 것 같다.

참조 자료

- [Java의 날짜와 시간 API](https://d2.naver.com/helloworld/645609)
- [Alias Bug Martin fowler](https://martinfowler.com/bliki/AliasingBug.html)
- [Value Object Martin fowler](https://martinfowler.com/bliki/ValueObject.html)
- [Domain-Driven Design의 적용-1.VALUE OBJECT와 REFERENCE OBJECT 1부](http://aeternum.egloos.com/1105776)
- [일급 컬렉션 (First Class Collection)의 소개와 써야할 이유](https://jojoldu.tistory.com/412)