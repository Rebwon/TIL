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

### 3장 요약

- 서비스 객체가 아닌 객체는 의존성이 아니라 값이나 값 객체를 받는다. 생성 시점에는 객체가 일관되게 행위를 하는 데 필요한 최소량의 데이터를 제공해야 한다. 생성자 인자가 어떤 식으로든 유효하지 않으면 생성자에서 이에 대한 예외를 일으켜야 한다.
- 기본 타입 인자를 (값) 객체 안으로 감싸는 것은 도움이 된다. 이렇게 하면 이런 값에 대한 유효성 확인 규칙을 재사용하기 쉽다. 또한 그 값의 타입(클래스)에 도메인별 이름을 지정행 코드에 더 많은 의미를 부여하게 된다.
- 서비스가 아닌 객체에서 생성자는 명명 가능한 생성자라고 하는 정적 팩토리 메서드여야 한다. 명명 가능한 생성자는 도메인별 이름을 코드에 도입하는 또 다른 기회를 제공한다.
- 생성자는 해당 단위 테스트에서 지정한 대로 객체가 행위를 하는 데 필요한 것 이상으로 데이터를 제공하지 않는다.
- 이런 규칙 대부분에 해당하지 않는 객체 타입이 DTO이다. 

### 불변 객체

도메인 개체(도메인 클래스 혹은 엔티티)를 제외한 나머지 모든 객체는 불변해야 한다.
- 변경 불가능 객체의 변경자는 변경한 복사본을 반환해야 한다.
- 변경 가능 객체의 변경자 메서드는 명령 메서드여야 한다.
  - 엔티티에 속성에 새로운 값을 할당하는 메서드가 void타입일 경우 command method이며 void 타입에 값을 변경하는 어떠한 메서드는 모두 명령형 메서드의 상징이다.
- 변경 불가능 객체의 변경자 메서드 이름은 서술형이어야 한다.
  - void moveLeft()보다는 Position toMoveLeft()가 불변객체에서는 더 서술적이며 적절한 이름이다. 또한 좋은 이름은 최대한 세부적이지 않으며, 일반적인 이름 대신 추상화된 이름이어야 한다. 예) withXDecreaseBy -> toMoveLeft

### 시스템 경계를 넘는 질의에는 추상화를 정의한다

외부 인프라 서비스 혹은 메일 서비스와 같은 애플리케이션 외부에서 발생되어지는 서비스에 대한 질의 메서드를 작성할 경우 적절하게 추상화하자.

아래의 코드는 적절한 추상화를 도입하는데 실패한 사례이다. 아래의 코드를 점진적으로 추상화를 도입하여 개선해보자.
```
class FixerApi {
    public ExchageRate exchageRateFor(Currency from, Currency to) {
        httpClient = new CurlHttpClient();
        response = httpClient.get(....);
        ...
        rate = (float) decoded.rates[to.asString()];

        return ExchangeRate.from(from, to, rate);
    }
}

class CurrencyConverter {
    private FixerApi fixerApi;

    public CurrencyConverter(FixerApi fixerApi) {
        this.fixerApi = fixerApi;
    }

    public Money convert(Money money, Currency to) {
        exchangeRate = this.fixerApi.exchangeRateFor(money.currency(), to);
        return money.convert(exchangeRate);
    }
}
```

HttpClient라는 추상화를 도입.
```
interface HttpClient {
    public Response get(url);
}

class CurlHttpClient implements HttpClient {
    // ..
}

class FixerApi {
    private HttpClient httpClient;

    public FixerApi(HttpClient httpClient) {
        this.httpClient = httpClient;
    }

    public ExchageRate exchageRateFor(Currency from, Currency to) {
        response = this.httpClient.get(....);
        ...
        rate = (float) decoded.rates[to.asString()];

        return ExchangeRate.from(from, to, rate);
    }
}
```

구체적인 구현 사항에 의존하지 않고 인터페이스에 의존하게 함으로써 구현 변경의 유연함을 더했다. 만약 다른 API로 전환할 때는 어떻게 될까? 다른 API에서 동일한 응답을 하지는 않을 수 있다. 저수준 구현 상세 내용을 제거하려면 추상적인 이름을 정의해야 한다.


```
interface ExchangeRates {
    public ExchangeRate exchangeRateFor(Currency from, Currency to);
}

class FixerApi implements ExchangeRates {
    private HttpClient httpClient;

    public FixerApi(HttpClient httpClient) {
        this.httpClient = httpClient;
    }

    public ExchageRate exchageRateFor(Currency from, Currency to) {
        response = this.httpClient.get(....);
        ...
        rate = (float) decoded.rates[to.asString()];

        return ExchangeRate.from(from, to, rate);
    }
}

class CurrencyConverter {
    private ExchangeRates exchangeRates;

    public CurrencyConverter(ExchangeRates exchangeRates) {
        this.exchangeRates = exchangeRates;
    }

    public Money convert(Money money, Currency to) {
        return money.convert(exchangeRateFor(money.currency(), to));
    }

    private ExchangeRate exchangeRateFor(Currency from, Currency to) {
        return this.exchangeRates.exchangeRateFor(from, to);
    }
}
```

마지막 개선으로, CurrencyConverter에 비공개 exchangeRateFor()는 이제 ExchangeRates 서비스에 대한 Proxy일 뿐이므로 기존 호출 모두를 인라인화 한다. 기존 클래스에 대한 인터페이스를 정의해 성공적인 추상화를 수행했다.

### 6장 요약

- 질의 메서드는 정보를 가져오는 데 사용할 수 있는 메서드다. 질의 메서드는 반환 값이 하나여야 한다. null을 반환하는 것보다는 널 객체나 빈 목록 같은 대안을 찾는 것이 좋다. 예외를 대신 일으킬 수도 있다. 질의 메서드는 될 수 있는 한 객체 내부를 노출하지 않아야 한다. 
- 요청할 모든 질문과 얻을 모든 답에 특정 메서드와 반환 값을 정의한다. 질문에 대한 답을 시스템 경계를 지나서만 얻을 수 있다면 이런 메서드에 추상화를 정의한다.
- 질의를 사용해 정보를 가져오는 서비스를 테스트할 때는 실제 호출을 테스트하지 말고, 직접 작성한 가짜와 스텁으로 대체한다.

### 9장 요약 (서비스 행위 변경하기)

- 서비스의 행위를 변경해야 한다고 느끼면 그 행위를 생성자 인자를 통해 설정할 수 있는 방법을 찾는다. 더 큰 논리를 대체해야 해서 이 방법을 선택할 수 없으면 의존성을 교환할 방법을 찾는다. 이 또한 생성자 인자로 전달한다.
- 변경할 행위가 아직 의존성으로 나타나지 않으면, 고수준 개념과 인터페이스라는 추상화를 도입해 추출한다. 그러면 변경 대신 대체할 수 있는 부분을 얻을 수 있다. 추상화는 행위를 구성하고 장식할 수 있는 능력을 제공하므로, 초기 서비스가 그 행위를 몰라도(또는 그 행위에 대해 초기 서비스를 변경하지 않고) 더 복잡한 행위를 이룰 수 있다.
- 메서드를 재정의해 서비스의 행위를 변경하는 데 상속을 사용하지 않는다. 항상 객체 구성을 사용하는 해결책을 찾는다. 상속한 모든 클래스는 마무리를 제대로 해야 한다. 즉 클래스를 final로 하고, 클래스의 공개 인터페이스가 아닌 모든 메서드와 속성을 private으로 한다.

### 10장

개체는 응용 프로그램의 도메인 개념을 나타낸다. 관련 데이터를 담고 있으며, 이 데이터와 연관된 유용한 행위를 제공한다. 객체 디자인 면에서, 이는 특정 개체 종류를 생성하는 도메인 별 이름을 사용할 있게 하므로, 정적 팩토리 메서드를 사용한 이름을 정의한다. 또한 개체에는 변경자(Setter 혹은 어떠한 상태 변경) 메서드도 있으며, 이는 개체의 상태를 변경하는 명령(Command) 메서드이다. 정보를 가져올 땐 일반적으로 Query 객체 혹은 특정 객체 종류에 위임한다. 

다음과 같은 특징을 갖는 객체는 개체다.
- 유일한 식별자가 있고
- 수명 주기가 있으며
- Repository에 저장함으로써 나중에 불러올 수 있고
- 정적 팩터리 메서드를 사용해 인스턴스를 만들거나 그 상태를 조작하는 방법을 사용자에게 제공하며
- 인스턴스를 만들거나 변경할 때 도메인 이벤트를 만들어낸다.

값 객체는 다음과 같은 특징을 갖는다.
- 변경할 수 없고
- 기본 타입 정보를 감싸며
- 도메인별 용어를 사용해 의미를 추가하고(예를 들어 int가 아니라 Year)
- 유효성 확인으로 제한을 가하며(예를 들어, 단순히 모든 문자열이 아니라 @가 있는 문자열)
- 개념과 관련된 유용한 행위와 유인자로 행동한다(예를 들어 Position.toTheLeft(int steps)).

Event Dispatcher의 특징
- 의존성을 주입한 변경 불가능 서비스이고
- 도메인 이벤트인 단일 인자를 받아들이는 메서드가 적어도 하나 있다.

#### 추상화(Interface, Abstract), 구체화(Concrete, Realize), 계층(Layer) 그리고 의존성(Dependency)

- 제어기는 구현체다
  - 흔히 특정 프레임워크와 결합하며 특정 Delivery 메커니즘에 맞게 구현된다. 제어기는 인터페이스가 없거나 필요 없다.
  대체 구현은 프레임워크를 전환할 때만 제공하면 된다.
- 응용 프로그램 서비스는 구현체다
  - 사용하는 응용 프로그램의 매우 특정한 사용 사례를 나타낸다. 사용 사례의 이야기가 바뀌면 응용 프로그램 서비스 자체도 바뀌므로
  인터페이스를 지니지 않는다.
- 개체와 값 객체는 구현체다
  - 이는 개발자가 도메인을 이해하는 특정한 결과다. 이 객체의 타입은 내내 진화한다. 이 객체에 대해서는 인터페이스를 제공하지 않는다. 
- Repository는 추상과 최소 한 개 이상의 구체화 구현으로 구성된다. Repository는 외부 세계와 접촉하고 연결하는 서비스이다. 따라서 추상화가 필요하다. 그리고 구현에서는 처리 방법에 관한 모든 저수준 상세 내용을 제공한다. 외부 세계와 연결하는 응용 서비스 역시도 마찬가지로 인터페이스로 추상화하고 구체화 구현체를 만들어야 한다.