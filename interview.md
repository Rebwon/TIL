## 각종 면접과 관련한 질문에 대한 답변 정리.

1. thread와 process의 차이를 설명할 수 있나?
   - 답변: 프로세스는 시스템의 자원을 할당 받아 실행 되는 프로그램입니다. 스레드는 프로세스 안에서 실행되는 여러 흐름의 단위입니다.
2. 내부 repository를 사용하는 이유는 뭐라고 생각하나?
   - 답변: Maven 중앙 레포지토리에 존재하지 않는 모듈을 사용해야 할때, 내부용 로컬 레포지토리를 만들게 되면 많은 개발자들의 중복적인
   작업이 줄어들게 됩니다.
3. gradle과 maven의 차이점은 무엇인가?
   - gradle이 maven보다 좋은점: https://kwonnam.pe.kr/wiki/gradle/from_maven
5. elastic-search 와 graph-QL에 대해 설명해보라.
6. 동적 스키마 설계시 고민할 점은 무엇일까?
7. 절차지향과 객체지향 개발의 차이점을 설명해보라.
   - 절차지향
     - waterfall 형식으로 코드를 읽어간다.
     - 데이터가 프로시저와 결합이 강하다.
     - 컴퓨터의 처리구조와 유사해 실행속도가 빠르다
     - 유지보수가 어렵다.
     - 디버깅이 어렵다.
     - 실행 순서가 바뀌면 아웃풋도 바뀐다.
   - 객체지향
     - 유지보수에 좋다.
     - 디버깅이 편하다.
     - 객체가 상태를 가지고 메세지를 보낸다.
     - 변경 부분을 잘 추상화한다면, 더 많은 요구사항을 쉽게 처리할 수 있다.
8. interceptor와 filter의 순서와 차이점은?
   - 차이점:
     - Filter는 Web Application에 등록을 한다.
     - interceptor는 Spring의 ApplicationContext에 등록 한다.
     - Filter는 Servlet에서 처리하기 전후를 다룰 수 있다.
     - Interceptor는 Handler를 실행하기전, Handler를 실행한 후, View를 렌더링한 후 등,
       Servlet 내에서도 메서드에 따라 실행 시점을 다르게 가져간다.
   - Interceptor
     - 작성해야할 전후처리 로직에서 예외를 전역적으로 처리하고 싶다면 Interceptor 사용이 좋다.
     - AOP 흉내를 낼 수 있다.
     - View를 렌더링하기 전에 추가 작업을 할 수 있다. 예를 들어 웹 페이지가 권한에 따라 GNB(Global
       Navigation Bar) 항목이 다르게 노출되어야 할 때 등의 처리를 할 수 있다.
   - Filter
     - ServletRequest 혹은 ServletResponse를 교체할 수 있다. 
9. transaction isolation level의 종류 및 특징은 무엇인가?
   - Isolation level
     - READ UNCOMMITED
     - READ COMMITED
     - REPEATABLE READ
     - SERIALIZABLE
   - 특징: https://joont92.github.io/db/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80-isolation-level/
10. JTA란 무엇인가?
   - 답변: Java Transaction API로 각 플랫폼마다 상이한 트랜잭션 매니저들과 애플리케이션들이 상호작용할 수 있는 인터페이스를 정의한 것입니다.
11. CDN과 AWS cloudfront의 차이점과 사용 이유를 설명해보라.
12. Static function의 특징은?
   - 답변: 
     - 클래스의 인스턴스 없이 호출이 가능하다. 
     - 유틸리티 함수를 만드는데 유용하다.
     - 상태가 없는 클래스이다.
     - 인스턴스 생성에 의존하지 않는다.
     - 메소드가 변화되지 않고, 오버라이딩 되지 않는다.
     - private 생성자로 인스턴스 생성을 제한해야 한다.
13. MD5, AES256, SHA256의 차이점과 각 암호화 방식에 대해 설명해보라.
14. CDC는 무엇이며 구현 방법은 무엇인가?
    - CDC는 Change Data Capture 데이터베이스 내 데이터에 대한 변경을 식별해 필요한
      후속처리(데이터 전송/공유 등)를 자동화하는 기술 또는 설계 기법이자 구조.
    - 구현 기법과 방식: https://specialscene.tistory.com/34
    - 자바 진영 JPA에서는 Spring Data Envers를 사용한다.
    - JPA에서 @Version으로 버저닝하는 것도 이에 포함
    - @LastModified 애노테이션을 붙여서 Time Stamp on rows 기법을 사용하는 것도 여기에 해당.
    - Event, Trigger를 사용할 수도 있으나, 애플리케이션 개발의 부담과 복잡도를 증가시킬 수 있다.
15. Fault-tolerant(무정지) 시스템으로 가기 위해 필요한 개발 방법에 대한 생각을 말해보라.