## 서비스

단일 객체 서비스: 서비스가 객체 하나로 구성되어있고 적절하게 분화하지 않음.

서비스 객체의 용도

- 경계 객체
- 컨트롤러
- Facade

무거운 서비스의 정리

- AOP를 사용해서 종적 관심사와 횡적 관심사 분리.
- 조합 메서드를 활용하고 DSL을 개발하라.
- 도메인 모델을 도입하라.
- 큰 서비스를 작은 클래스로 분해하고 위임하라.
- 높은 응집도와 낲은 결합도를 갖도록 설계하라.

Facade란?

여러개의 서브 시스템을 하나의 서비스로 추상화하여, 확장성을 고려한
디자인 패턴이다. 

만약 이 클래스가 바뀐다면 얼마나 많은 코드가 바뀌어야 하는가와 같은
확장성 문제를 고려한 패턴이다.

도메인 주도 설계에서 응용 영역에 응용 서비스는 표현 영역과 도메인 영역을 연결하는 창구인 파사드(Facade)역할을 한다.
응용 서비스는 주로 도메인 객체 간 흐름 제어만을 위해 존재하므로 단순한 형태를 갖는다.

응용 서비스의 구현은 구분되는 기능 별로 구현 클래스를 통해 구현하는 것이 좋다.
한 응용 서비스에서 많은 기능을 구현할 경우 복잡도가 증가하고 연관성이 적은 객체가 서비스에 밀집할 가능성이 높다.

이는 결과적으로 관련 없는 코드가 뒤섞여서 코드를 이해하는데 방해가 될 수 있다.

서비스 영역은 표현 영역에 절대 의존하면 안된다. 영역 별 분리가 레이어 아키텍처에서 가장 중요하다.