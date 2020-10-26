## Object Design Style Guide

### 의존성과 설정값은 생성자 인자로 주입한다.

모든 의존성을 생성자를 통해 주입하면, 인스턴스 생성 시 즉시 서비스를 실행할 수 있다. 또한 추가 설정은 필요하지 않으며, 뜻하지 않게 의존성을 빠뜨리는 일도 없다. 

또한 스프링에서 @ConfigurationProperties를 사용하여 설정값을 주입하거나 할 때, AppConfig와 같은 설정 객체를 주입하는게 아닌, 필요한 설정 값만 주입하자.

짝 지어 다니는 설정 값을 따로따로 주입하면 자연스런 응집력을 망가뜨린다.

```java
class ApiClient {
    // String(username, password)를 가진 객체
    private Credentials cridentials;

    public ApiClient(Credentials cridentials) {
        this.cridentials = cridentials;
    }
}
```

### 작업 관련 데이터는 생성자 인자가 아니라 메서드 인자로 전달해야 한다.

```
EntityManager em = new EntityManager(user);
em.save();

EntityManager em = new EntityManager(comment);
em.save();  // wrong case

EntityManager em = new EntityManager();
em.save(user); // good case
```
해야 하는 모든 작업에 새로운 인스턴스를 생성해야 하기 때문에 EntityManager는 유용하지 않다.

### 2장 요약

- 서비스를 만들 때는 모든 의존성과 설정값을 생성자 인자로 제공해야 하며 한 번에 생성해야 한다. 모든 서비스 의존성은 명시적이어야 하며 객체로 주입해야 한다. 모든 설정값은 유효해야 한다. 생성자가 어떤 식으로든 유효하지 않은 인자를 받으면 예외를 일으켜야 한다.
- 서비스는 생성 후 변경 불가능해야 하며, 어떤 메서드를 호출하든 그 행위가 바뀌지 않아야 한다.
- 응용 프로그램의 모든 서비스를 결합하면 종종 서비스 컨테이너로 관리하는, 크고 변경 불가능한 객체 그래프를 형성한다. 제어기는 이 그래프의 진입점이다. 서비스는 인스턴스를 한 번만 만들어 여러 번 재사용할 수 있다.

### 값 객체

값 객체라는 새로운 객체로 값을 감싸는 것은 유효성 확인 논리를 반복하지 않게 하는 것과 메서드가 기본 타입값을 받아들인다는 것을 아는 즉시, 이에 대해 클래스를 도입하는 것을 고려해야 한다.

유사한 두개의 값 객체는 복합 값으로 만들어서 사용하자.

값 객체와 같은 객체 타입을 더 추가할수록 더 많이 타이핑해야 한다. 그럼에도 필요한가?

1. 객체로 감싼 데이터의 유효성 확인을 확실히 할 수 있다.
2. 객체는 일반적으로 자신의 데이터를 이용하는 추가적이며 의미 있는 행위를 노출한다.
3. 객체는 함께 속한 값을 함께 유지할 수 있다.
4. 객체는 클라이언트가 상세 구현 내용에 접근하지 못하게 하는 데 도움이 된다.

아래와 같이 기본 타입을 하나하나 객체로 생성하기 버겁다면, Helper 메서드를 사용하자.
```
// before
money = new Money(new Amount(100), new Currency('USD'));

// after
money = Money.create(100, 'USD');
```

### Assert로 생성자 인자 유효성을 검사한다.

기본적으로 아래와 같은 조건으로 메서드 시작 전에 점검을 한다. 이러한 점검을 사전 조건 확인이라고 부르는데, 확인 점검 절차를 여러 곳에서 수행하는 경우가 많으므로, spring의 Assert와 같은 라이브러리를 사용하여 하는 것이 바람직하다.
```
if(somethingWrong()) {
    throw new Exception();
}
```

### 의존성을 주입하지 말고 선택적인 메서드 인자로 전달한다

서비스에는 의존성이 있을 수 있으며 이는 생성자 인자로 주입한다. 하지만 다른 객체에는 값, 값 객체 또는 이런 것의 리스트 등 어떤 의존성도 주입하면 안된다. 값 객체가 어떤 작업을 수행하는 데 서비스가 필요하다면 아래와 같이 선택적 메서드 인자로 주입하자.

```
class Money {
    private ExchangeRateProvider exchageRateProvider; // 서비스를 값 객체에 주입하면 안된다.
    ...
}

class Money {
    public void something(ExchangeRateProvider exchageRateProvider) { // 이런 식으로 선택적으로 사용하자.
        ...
    }
}
```

또 다른 구현 방법은 다음과 같다.
```
class ExchangeRate {
    public ExchangeRate(Currency from, Currency to, Rate rate) {
        ...
    }

    public Money convert(Amount amount) {
        ...
    }
}

money = new Money(...);
exchangeRate = exchageRateProvider.getRateFor( // exchageRate을 먼저 가져온다.
    money.currency(),
    targetCurrency
);
converted = exchageRate.convert(money.getAmount()); // 그런 다음 금액 변환하는데 사용한다
```

한 번 더 바꾸면, 아래와 같이 Money의 내부 Amount는 제외하고 Currency 객체만 드러내는 해결책으로 바꿀 수 있다.
```
class Money {
    public Money convert(ExchageRate exchageRate) {
        return new Money(exchageRate.rate().applyTo(this.amount), exchageRate.targetCurrency());
    }
}

money = new Money(...);
exchangeRate = exchageRateProvider.getRateFor(
    money.currency(),
    targetCurrency
);
converted = money.convert(exchageRate); 
```

또 다른 방법으로는 서비스를 메서드 인자로 전달해야 한다는 점이 그 행위 대신 서비스를 구현해야 한다는 암시일 수 있다. 따라서 서비스를 생성하고 제공한 Amount와 Currency 객체에서 관련 정보를 수집하여 그 일을 서비스가 하도록 하는 게 낫다.
```
class ExchageService {
    private ExchangeRateProvider exchageRateProvider;

    public Money convert(Money money, Currency targetCurrency) {
        exchangeRate = this.exchageRateProvider.getRateFor(money.currency(), targetCurrency);
        return new Money(exchageRate.rate().applyTo(money.amount()), targetCurrency);
    }
}
```

어느 해결책을 선택하느냐는 행위와 데이터를 얼마나 가까이 두기 원하는지, Money와 같은 객체가 환율 정보를 아는 것이 너무 과하다 생각되는지, 또는 객체 내부를 노출하는 것을 얼마나 피하고 싶은지에 달렸다.

### 정적 팩토리 메서드를 사용할 때 도메인 별 개념을 사용하여 이름을 짓자.

from, of와 같은 명명 규칙도 좋지만, 실제 도메인의 개념인 place, create, write와 같은 더 한정적인 용어를 사용하자.