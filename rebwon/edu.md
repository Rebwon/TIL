## 내가 정리하는 자바 웹 백엔드 학습 순서

지금까지는 얕고 넓은 수준의 지식이었다면, 이제는 깊이를 더해가보자.

깊이가 더해졌다고 생각되어지는 부분은 *로 표시를 하자.

### Java 학습 순서

- 자바 기초 쌓는 순서
  - 기본적인 문법을 공부한다.
    - 변수
    - 연산자
    - 조건문 반복문
    - 배열
  - 객체지향 프로그래밍 학습
    - 클래스, 인스턴스, 메서드
    - static, void, public, private 등의 키워드
    - 오버로딩, 오버라이딩
    - 생성자, super(), this()
    - 상속
    - 추상 클래스
    - 다형성
    - instanceof
    - 인터페이스
    - 내부 클래스
    - 익명 클래스
  
- 자바 중급으로 가는 순서
  - 예외 처리
  - 컬렉션 프레임워크 학습
  - java.lang 패키지 학습
  - 자바 8 람다, 메서드 레퍼런스, 스트림 API 학습
  - 쓰레드, IO 학습
  - 애노테이션 학습
  - 네트워크 학습
  
- 자바 고급
  - 제네릭
  - 리플렉션
  - JVM 구조와 클래스 로더 동작 이해
  - 이펙티브 자바 학습
  
여기까지가 자바를 그래도 해봤다하는 정도...

### Java를 넘어 OOP로..

- OOP를 제대로 학습하려면?
  - 디자인 패턴 학습
    - GoF 말고도 J2EE 패턴 숙지
  - SOLID, GRASP 등의 개념 숙지
  - 리팩토링, TDD 등의 기본기를 다지기

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
      - AOP는 너무 빡세..
    - Null Safety
  - 스프링 MVC
    - 서블릿 기초와 리스너, 필터 학습
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
  - 자바 기본 JDBC를 학습(중복적인 코드에 빡이 친다.)
  - Spring Data JDBC!!
    - [benelog님 정리](https://github.com/benelog/spring-jdbc-tips)

- JPA
  - 난이도는 내려갈수록 기하급수적으로 상승...
  - 자바 ORM 표준 JPA 프로그래밍 서적 학습
  - QueryDSL 학습
  - Spring Data JPA 학습
  - Hibernate 학습
  - 그외에 Requery라는 것도 있다..
  
  
### OOP를 넘어 DDD로..

- DDD는 또 어떻게..?