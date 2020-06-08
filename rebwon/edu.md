## 내가 정리하는 자바 웹 백엔드 학습 순서

Lisense: [Rebwon](github.com/Rebwon)

### Java 기초 단계

- 변수 *
- 연산자 *
- 조건문 반복문 *
- 배열 *

### Java 중급 단계

- 예외 처리 *
- 컬렉션 프레임워크 학습
- java.lang 패키지 학습 *
- 자바 8 람다, 메서드 레퍼런스, 스트림 API 학습 *
- 애노테이션 학습 *

### Java 고급 단계
  
- 제네릭 
- 리플렉션
- 쓰레드, IO
- 네트워크
- JVM 구조와 클래스 로더 동작 이해 *
- 이펙티브 자바 학습

### OOP 기본 단계

- 클래스, 인스턴스, 메서드 *
- static, void, public, private 등의 키워드 *
- 오버로딩, 오버라이딩 *
- 생성자, super(), this() *
- 상속 *
- 추상 클래스 *
- 다형성 *
- instanceof *
- 인터페이스 *
- 내부 클래스 *
- 익명 클래스 *

### OOP 심화 단계

- 디자인 패턴 학습 *
  - GoF 말고도 J2EE 패턴 숙지
- SOLID, GRASP 등의 개념 숙지 *
- 리팩토링, TDD 등의 기본기를 다지기 *

### Spring 학습 순서

- 야생형
  - 무작정 만들어보고 아래에 방법을 하나씩 숙지
- 이론형
  - Step By Step으로 하나씩
  - 스프링 Core
    - IOC 컨테이너 학습
    - Resource, Validation
    - 데이터 바인딩
    - SpEL
    - AOP
    - Null Safety
  - 스프링 MVC
    - 서블릿 기초와 리스너, 필터 학습 *
    - DispatcherServlet 동작 과정과 로딩되는 빈들을 숙지
    - WebMvcConfigurer
    - 핸들러 인터셉터
    - 리소스 핸들러
    - 아규먼트 리졸버
    - 메세지 컨버터
    - 그외에 MVC 익셉션 처리
  - 스프링 부트
    - 자동 설정이 어떤 방식인지 학습
    - 그밖에 것들..
  - 스프링 Hateoas
  - 스프링 Reactive 
    - R2DBC
    - NON BLOCKING I/O
    - Mono, Flux

### JPA, JDBC 학습 순서

- JDBC
  - 자바 기본 JDBC를 학습 *
  - Spring Data JDBC
    - [benelog님 정리](https://github.com/benelog/spring-jdbc-tips)

- JPA
  - 자바 ORM 표준 JPA 프로그래밍 서적 학습 *
  - QueryDSL 학습
  - Spring Data JPA 학습 *
  - Hibernate 학습
  - 그외에 Requery
  
  
### DDD

- BC
- VO
- UC
- CQRS
- Repository
- factory
- aggregate
- event
- event sourcing