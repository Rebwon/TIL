Dependency Injection in .net

## Chapter 1

- 의존성 주입에 대한 오해입니다.
- 의존성주입의 목적입니다.
- 의존성 주입의 이점입니다.
- Dependency Injection 적용 시기입니다.

### 유지보수 가능한 코드

DI는 그 자체가 목표가 아니라 목적 달성을 위한 수단입니다.

궁극적으로 대부분의 프로그래밍 기법의 목적은 소프트웨어를 최대한 효율적으로 제공하는 것입니다. 그것의 한 측면은 유지보수 가능한 코드를 작성하는 것입니다.

코드를 유지보수하는 여러 방법 중 하나는 느슨한 결합을 사용하는 것입니다.

Program to an interface, not an implementation. 

"구현이 아닌, 인터페이스에 프로그래밍합니다"

협력하는 클래스들은 필요한 서비스를 제공하기 위해 인프라에 의존해야 합니다.

### Unlearning DI

- DI is only relevant for late binding.
- DI is only relevant for unit testing.
- DI is a sort of Abstract Factory on steroids.
- DI requires a DI CONTAINER.

위와 같은 오해를 하고 있다면, 책을 읽기 전 마음을 정리합니다.

지연 바인딩은 DI에 의해 사용되지만, 지연 바인딩에만 적용된다고 가정하는 것은 DI에 대한 훨씬 더 넓은 시야를 좁게 보는 것입니다.

이러한 맥락에서 지연 바인딩은 코드를 다시 컴파일하지 않고 응용 프로그램의 일부를 교체하는 기능을 의미합니다.

또 다른 예로는 둘 이상의 데이터베이스 엔진에서 실행할 수 있습니다. 이 기능을 지원하기 위해 나머지 응용 프로그램은 인터페이스를 통해 데이터베이스와 통신합니다. 코드 베이스는 Oracle, MySQL에 대한 엑세스를 제공하기 위해 이 인터페이스의 다양한 구현을 제공할 수 있습니다.

DI가 이러한 시나리오에만 관련된다는 것은 일반적인 오해입니다. DI는 이러한 시나리오를 가능하게 하기 때문에 이해할 수 있지만, 오류는 관계가 대칭적이라고 생각하는 것입니다. DI가 지연 바인딩을 지원한다고 해서 지연 바인딩 시나리오에서만 관련이 있는 것은 아닙니다. 지연 바인딩은 DI의 많은 측면 중 하나에 불과합니다.

단위 테스트 역시 지연 바인딩과 마찬가지입니다. DI를 단위 테스팅하기 위해서라고만 본다면 지연 바인딩과 DI가 대칭적이라는 오해와 같습니다.

가장 위험한 오류는 DI가 우리가 필요로 하는 의존성의 인스턴스를 만드는 데 사용할 수 있는 범용적인 추상 팩토리라고 이해하는 것입니다.

DI를 서비스 로케이터라고 생각했다면, 범용 팩토리라는 것을 알아야 합니다. DI는 서비스 로케이터의 반대입니다. DI는 코드를 구조화하여 의존성을 반드시 요청할 필요가 없도록 하는 방법입니다. 오히려, 우리는 클라이언트에게 의존성들을 공급하도록 강요합니다.

DI 컨테이너는 애플리케이션을 연결할 때 구성 요소를 쉽게 구성할 수 있는 옵션 라이브러리지만, 전혀 필요하지 않습니다. DI 컨테이너 없이 애플리케이션을 구성하는 것을 POOR MAN'S DI라고 합니다.

DI에 DI 컨테이너가 필요했다고 생각한다면 이는 DI 컨테이너를 사용해야 하는 또 다른 개념입니다. DI는 원칙과 패턴의 집합이며, DI 컨테이너는 유용하지만 선택적인 도구입니다.

DI는 최종 목표가 아닙니다. 목적을 위한 수단입니다. DI는 Loose Coupling을 가능하게 하며, Loose Coupling은 코드 유지보수의 용이성을 높여줍니다. 

인터페이스 및 DI 프로그래밍의 이점을 설명할 때, 한 서비스를 다른 서비스로 바꿀 수 있는 기능은 대부분의 사람들에게 가장 일반적인 이점입니다. 따라서 이러한 이점만 염두에 두고 단점과 단점을 비교하려고 합니다.

느슨한 결합을 통해 이점을 얻을 수 있습니다. 각 혜택은 항상 제공되지만 상황에 따라 다르게 평가됩니다.

- 지연 바인딩
  - 서비스는 다른 서비스로 대체 가능합니다.
  - 표준 소프트웨어에서는 유용하지만 런타임 환경이 잘 정의된 엔터프라이즈 애플리케이션에서는 그렇지 않을 수 있습니다.
- 확장성
  - 코드는 명시적으로 계획되지 않은 방법으로 확장 및 재사용할 수 있습니다.
  - 항상 가치가 있습니다.
- 병렬 개발
   - 코드 개발을 병렬로 합니다.
   - 크고 복잡한 애플리케이션에서 유용하며, 작고 단순한 애플리케이션에서는 그다지 유용하지 않습니다.
- 유지 보수성
  - 책임이 명확히 정의된 클래스는 유지하기가 더 쉽습니다.
  - 항상 가치가 있습니다.
- 단위 테스트
  - 클래스는 단위 테스트가 가능해집니다.
  - 단위 테스트를 수행하는 경우에만 유용합니다(실제로 수행해야 함).

아래는 Hello World와 유사하게 Hello DI의 예제 코드입니다.

```c#
private static void Main()
{
    IMessageWriter writer = new ConsoleMessageWriter();
    var salutation = new Salutation(writer);
    salutation.Exclaim();
}
```

```c#
public class Salutation
{
    private readonly IMessageWriter writer;
    public Salutation(IMessageWriter writer)
    {
        if (writer == null)
        {
            throw new ArgumentNullException("writer");
        }
        this.writer = writer;
    }
    public void Exclaim()
    {
        this.writer.Write("Hello DI!");
    }
}
```

```c#
public interface IMessageWriter
{
    void Write(string message);
}

public class ConsoleMessageWriter : IMessageWriter
{
    public void Write(string message)
    {
        Console.WriteLine(message);
    }
}
```

```c#
IMessageWriter writer = new ConsoleMessageWriter();
```

위와 같은 코드는 구현 클래스 인스턴스를 하드 코딩하여 새 IMessageWriter 인스턴스를 생성했기 때문에 늦은 바인딩을 사용하지 않았습니다.

늦은 바인딩을 사용하도록 설정하려면 코드 줄을 다음과 같은 것으로 바꿀 수 있습니다.

```c#
var typeName = ConfigurationManager.AppSettings["messageWriter"];
var type = Type.GetType(typeName, true);
IMessageWriter writer = (IMessageWriter)Activator.CreateInstance(type);
```

응용 프로그램 구성 파일에서 Type 이름을 꺼내고 이 파일에서 Type 인스턴스를 생성하면 Reflection을 사용하여 컴파일 시 구체적인 Type을 몰라도 IMessageWriter 인스턴스를 생성할 수 있습니다.

애플리케이션 구성 파일에서 다음을 설정합니다.

```xml
<appSettings>
 <add key="messageWriter"
 value="Ploeh.Samples.HelloDI.CommandLine.ConsoleMessageWriter, HelloDI" />
</appSettings>
```

인증된 사용자만 메시지를 쓰도록 허용하여 Hello DI 예제의 보안을 강화하려고 합니다. 다음 목록에서는 기존 기능을 변경하지 않고 해당 기능을 추가하는 방법을 보여 줍니다. 즉, IMessageWriter 인터페이스의 새 구현을 추가합니다.

```c#
public class SecureMessageWriter : IMessageWriter
{
    private readonly IMessageWriter writer;
    
    public SecureMessageWriter(IMessageWriter writer)
    {
        if (writer == null)
        {
            throw new ArgumentNullException("writer");
        }
        this.writer = writer;
    }

    public void Write(string message)
    {
        if (Thread.CurrentPrincipal.Identity.IsAuthenticated)
        {
            this.writer.Write(message);
        }
    }
}
```

사용 가능한 클래스를 이전과 다르게 구성하면 되므로 기존 코드를 변경해야 하는 유일한 장소는 Main 메서드입니다.

```c#
IMessageWriter writer = new SecureMessageWriter(new ConsoleMessageWriter());
```

안정적인 의존성의 기준 
- 클래스 또는 모듈이 이미 있습니다.
- 새 버전에는 변경 내용이 포함되어 있지 않을 것으로 예상합니다.
- 해당 유형에는 결정론적 알고리즘이 포함되어 있습니다.
- 클래스나 모듈을 다른 모듈로 교체할 필요가 없습니다.

아이러니하게도 DI 컨테이너 자체는 모든 기준에 적합하기 때문에 안정적인 의존성을 유지하는 경향이 있습니다. 특정 DI 컨테이너를 기반으로 애플리케이션을 결정하면 애플리케이션의 전체 수명 동안 해당 선택에 고립될 위험이 있습니다. 이러한 이유로 컨테이너 사용을 애플리케이션의 진입점인 메인 메서드로 제한해야 합니다.

애플리케이션에 응용 프로그램 구성 요소를 도입하는 것은 추가 작업이므로 필요할 때만 해야 합니다. 

안정성이 있는 의존성 (제어가 가능한)
- ex) Java의 List, Map 등 기본적으로 모든 언어에 베이스를 두는 라이브러리들.

휘발성이 있는 의존성 (제어가 불가능한)
- ex) Java의 LocalDateTime, Random 과 같은 라이브러리

휘발성 의존성은 DI의 핵심입니다. 애플리케이션에 응용 프로그램 구성 요소를 도입하는 것은 안정적인 의존성이 아니라 휘발성이 있는 의존성을 위한 것입니다.

DI의 중요한 요소는 다양한 책임을 세분화하는 것입니다. 우리가 클래스에게 가져가는 한 가지 책임은 의존성의 인스턴스를 만드는 것입니다.

클래스가 의존성에 대한 제어 권한을 포기함에 따라 특정 구현을 선택하기 위한 결정 이상의 것을 포기하게 됩니다. 

개발자로써 우리는 의존성을 사용하는 클래스에서 해당 제어권을 제거하여 제어가능함을 확보합니다. 이것은 단일 책임 원칙을 적용하는 것입니다. 이러한 분류는 의존성이 생성되는 방식에 관계없이 주어진 책임 영역만 다루어야 합니다.

DI는 의존성을 균일한 방식으로 관리할 기회를 제공합니다. 소비자가 직접 의존성의 인스턴스를 생성하고 설정할 때, 다른 소비자가 수행하는 방식과 일치하지 않을 수 있습니다. 우리는 의존성을 중앙 집중식으로 관리할 방법도 없고, cross-cutting 문제도 해결할 방법이 없습니다. DI를 통해 각 의존성 인스턴스를 가로채 소비자에게 전달되기 전에 조치를 취할 수 있습니다.

확장성, 늦은 바인딩 및 병렬 개발의 이점을 얻으려면 클래스를 애플리케이션으로 구성할 수 있어야 합니다. 아래 그림 참조. 이러한 객체 구성은 종종 애플리케이션에 DI를 도입하는 가장 중요한 동기가 됩니다. 처음에 DI는 Object Composition의 동의어였습니다. 

![objectcomposition](/image/oc.png)

DI는 객체 구성 문제를 해결하기 위한 일련의 패턴으로 시작되었지만, 이후 개체의 수명 및 인터셉션으로 용어가 확장되었습니다. 오늘날의 DI는 이 세가지를 모두 일관된 방식으로 포괄한다고 생각합니다.

DI는 설계 원리와 패턴의 모음에 불과합니다. 툴과 기술보다는 코드를 생각하고 설계하는 방법에 더 중점을 둡니다. 느슨한 커플링과 DI에 대한 중요한 점은 코드 베이스의 모든 곳에 있어야만 효과적으로 사용할 수 있다는 것입니다.

DI의 목적은 코드를 유지보수하기 위한 것입니다. Hello World 수준의 소규모 코드 베이스는 크기가 작기 때문에 본질적으로 유지보수가 가능합니다. 이것이 바로 단순한 예에서 DI가 오버엔지니어링처럼 보이는 이유입니다. 코드 기반이 커질수록 이점을 더 잘 볼 수 있습니다.

## Chapter 2

느슨하게 결합된 코드의 핵심은 복잡성을 효율적으로 처리하는 것입니다.