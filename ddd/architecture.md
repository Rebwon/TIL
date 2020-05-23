## Architecture

- Web 기반 Accounting 시스템의 아키텍처에서 주목할 부분
  - Accounting 시스템?
  - Web 시스템?

- SW 아키텍처
  - Accounting issue를 드러내야 한다.
  - Web에 대해서는 거의 언급하지 않아야 한다.

- 하지만 대개의 Web 시스템은 반대
  - Web issue에 대해서 고함치고 있다.
  - 비즈니스 의도에 대해서는 거의 언급하지 않음.
  
### Web System에 만연한 MVC Architecture

- View/Controller : 강하게 HTML과 연관
- Model : Controller에 강하게 연관
- View/Controller : 강하게 모델에 coupling
  - 모델의 구조에 많은 영향을 미친다. 
  
### Accounting System Architecture

- Architecture 변경 없이 delivery 메커니즘을 변경할 수 있어야 한다.
  - 동일한 시스템을 웹과 콘솔에 deliver할 수 있어야 한다.
  - 이 두 시스템의 아키텍처는 동일해야 한다.
  
### Use Cases

- screen, button, field 등과 같은 웹(delivery 메커니즘)과 같은 것들은
언급하지 않음
- 시스템으로 들어가는 데이터, 커맨드와 시스템이 응답하는 것만 언급
- Delivery 메커니즘과 무관한 아키텍처를 가지려면 delivery 메커니즘과 무관한
use case로 시작해야 한다.
- Use case의 응답도 주목
  - orderId는 완전하게 delivery 메커니즘과 무관
  - human clerk은 orderId를 볼 필요가 없다
  - 반면 delivery 메커니즘은 팝업을 띄워서 사용자에게 항목을 주문에 추가하라고 요청할 수 있다.

Use Case는 입력 데이터를 해석하여 출력 데이터를 생성하는 필수 알고리즘

Use Case를 구현하는 객체를 생성할 수 있다는 것을 의미

Use Case는 primary course와 예외 상황 두가지 모두 존재함

### Use Case Algorithm

- 다른 비즈니스 객체들(Customer, Order)을 언급
- 알고리즘 : use case 정의, 비즈니스 규칙을 내포
- 하지만 이런 비즈니스 규칙은 Customer나 Order 객체에 속하지 않는다.
- 그럼 어디에 비즈니스 규칙을 내포시킬 것인가
- 어떤 객체에 위치시킬 것이며 Use Case 객체를 아키텍처의 어디에 위치시킬 것인가?
- 어떻게 우리의 시스템을 partition해서 use case가 central organizing principle이 되게 할 것인가

### Use Cases and Partitioning

- 시스템의 행위를 use case로 기술한다.
  - Use case에서 application 특화된 행위를 interactor 객체로 캡쳐한다.
  - Application 무관한 행위를 entity 객체로 캡쳐하고 interactor로 제어한다.
  - UI 종속적인 행위는 Boundary 객체로 캡쳐하여 interactor와 커뮤니케이션한다.
  
### Database

- 비즈니스 객체가 아니라 Data Structure를 포함
- 데이터가 저장되는 방식은 Interactor나 Entity가 원하는 방식이 아님
- 그러므로 DB와 Entity 간의 Boundary Layer를 제공해야 함
  - DIP!

### Conclusion

Architecture는 툴이나 프레임워크에 기반하지 않는다.

좋은 Architecture
- 툴이나 프레임워크에 대한 결정을 아주 오랫동안 미룸
- 이행되지 않은 결정의 갯수를 최대화
- delivery 메커니즘에 의존하지 않고, 노출시키지 않고 숨김
- 시스템의 모양을 보면 web 시스템인지 여부를 알 수 없어야 함
- 시스템의 use case는 주요한 추상화이고 시스템 아키텍처를 구성하는 핵심적인 원칙
- 아키텍처를 보면 UI가 아니라 시스템의 의도를 볼 수 있어야 함

나의 정리 : 

Delivery 메커니즘에 종속적인 시스템 애플리케이션 구조를 만들면 안된다.

Decouple한 애플리케이션 구조를 만들려면 Interface를 활용해 Boundary 객체(서비스 레이어)를 만든다.
또한 DB와 Decouple한 애플리케이션 구조에서는 Boundary Layer를 제공할 Repository를 만든다.

위와 같이 설계하고 개발할 때 애플리케이션이 안전하게 보호되고 Decouple한 상태로 Isolation을 보장하며 
Use Case를 만족할 수 있다.