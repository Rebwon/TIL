## Spring History 정리

- 스프링 0.9 버전: 빈 구성 기반, AOP 지원, JDBC 추상화 프레임워크, 추상 트랜잭션 지원 등을 제공한다. 해당 버전에 대한 레퍼런스 문서는 없지만,
소스포지(SourceForge)에서 기존 소스와 문서를 찾아볼 수 있다.

- 스프링 1.x 버전
  - 스프링 코어: 빈 컨테이너와 유틸리티 지원
  - 스프링 컨텍스트: ApplicationContext, UI, 유효성 검증(validation), JNDI, 엔터프라이즈 자바빈즈(Enterprise JavaBeans, EJB), 리모팅(remoting), 메일 지원
  - 스프링 DAO: 트랜잭션 인프라, JDBC(Java Database Connectivity), DAO(Data Access Object) 지원
  - 스프링 ORM: 하이버네이트(Hibernate), 아이바티스(iBATIS), JDO(Java Data Objects) 지원
  - 스프링 AOP: AOP 얼라이언스 호환(Alliance–compliant) 관점 지향 프로그래밍(aspect-oriented programming, AOP) 구현
  - 스프링 웹: 멀티파트 처리, 서블릿 리스너를 통한 컨텍스트 초기화, 웹 애플리케이션 컨텍스트와 같은 기본적인 통합 기능
  - 스프링 웹 MVC: 웹 기반 MVC(Model-View-Controller) 프레임워크

- 스프링 2.x 버전: 스프링 컨텍스트 모듈은 스프링 코어 모듈에 포함되었고, 모든 스프링 웹 컴포넌트는 하나의 항목으로 구성됨.
  - DTD(Document Type Definition) 형식이 아닌 새로운 XML 스키마 기반 구성을 사용해 보다 쉽게 XML을 구성할 수 있습니다. 빈 정의와 AOP, 선언적 트랜잭션의 기능이 크게 개선됐습니다.
  - 웹과 포털 사용을 위한 새로운 빈 스코프(Scope) 지원(요청, 세션, 전역 세션 스코프)
  - AOP 개발을 위한 @AspectJ 애너테이션 지원
  - 자바 퍼시스턴스 API(Java Persistence API, JPA) 추상화 레이어
  - 비동기 JMS 메시지 기반 POJO(Plain Old Java Objects)2를 완벽하게 지원
  - 자바 5+를 사용할 때 SimpleJdbcTemplate을 포함한 JDBC 단순화
  - JDBC 네임드 파라미터(named parameter) 지원(NamedParameterJdbcTemplate)
  - 스프링 MVC 폼(Form) 태그 라이브러리
  - 포틀릿 MVC 프레임워크 도입
  - 동적 언어 지원. 제이루비(JRuby), 그루비(Groovy), 빈셸(BeanShell)로 빈을 작성할 수 있습니다.
  - JMX에서 MBean 등록 제어와 알림 지원
  - 스케줄링 작업을 위한 TaskExecutor 추상화 도입
  - @Transactional, @Required, @AspectJ에 대한 자바 5 애너테이션 지원
  
- 스프링 2.5.x 버전
  - 새로운 @Autowired 구성 애너테이션과 JSR-250 애너테이션(@Resource, @PostConstruct, @PreDestroy) 지원
  - 새로운 스테레오타입 애너테이션: @Component, @Repository, @Service, @Controller 지원
  - 스테레오타입 애너테이션으로 구성된 클래스 감지 및 와이어링을 위한 자동 클래스 경로 검색(classpath-scanning) 지원
  - 새로운 빈 포인트컷 요소와 AspectJ 로드타임 위빙(load-time weaving)을 포함하는 AOP 기능 개선
  - 완전한 웹스피어 트랜잭션 관리 지원
  - 애너테이션을 통해서 웹 요청을 처리하기 위해 스프링 MVC @Controller 애너테이션과 @RequestMapping, @RequestParam, @ModelAttribute 애너테이션 추가
  - 타일즈(Tiles) 2 지원
  - JSF 1.2 지원
  - JAX-WS 2.0/2.1 지원
  - 스프링 TestContext 프레임워크의 도입. 이 테스트 프레임워크를 통해 애너테이션 기반의 통합 테스팅 지원
  - JCA 어댑터로 스프링 애플리케이션 컨텍스트를 배포하는 기능
  
- 스프링 3.0.x 버전: 자바 5를 기반으로 하는 스프링의 첫 번째 버전으로 제네릭, varargs 등의 자바 5 기능을 최대한 활용하도록 설계됨. 자바 기반의 @Configuration 모델이 도입됨. 프레임워크
모듈 JAR 당 하나의 소스 트리로 개별적으로 관리되도록 변경됨.
  - 제네릭, varargs 및 기타 개선 사항 등에 대한 자바 5 기능 지원
  - Callable, Future, ExecutorService 어댑터, ThreadFactory 통합에 대한 완전한 지원
  - 모듈 JAR 당 하나의 개별적인 소스 트리를 갖도록 프레임워크 모듈을 관리
  - 스프링 표현식 언어(SpEL) 도입
  - 코어 자바 구성 기능과 애너테이션 통합
  - 범용 타입 변환 시스템 및 필드 서식 시스템 지원
  - 포괄적인 REST 지원
  - 스프링 MVC에 새로운 MVC XML 네임스페이스와 @CookieValue 및 @RequestHeaders와 같은 애너테이션 추가
  - 유효성 검증 기능 향상과 JSR-303(빈 유효성 검증) 지원
  - @Async/@Asynchronous 애너테이션, JSR-303, JSF 2.0, JPA 2.0 등의 자바 EE 6에 대한 조기 지원
  - HSQL, H2, Derby 등의 임베디드 데이터베이스 지원
  
- 스프링 3.1.x 버전
  - 새로운 캐시 추상화
  - XML로 빈을 정의하는 것과 동일한 @Profile 애너테이션에 대한 빈 정의 프로파일 지원
  - 프로퍼티(Property) 통합 관리를 위한 환경 추상화
  - 스프링에서 사용하는 XML 네임스페이스 요소와 동등한 역할을 하는 @ComponentScan, @EnableTransactionManagement, @EnableCaching, @EnableWebMvc, @EnableScheduling, @EnableAsync, @EnableAspectJAutoProxy, @EnableLoadTimeWeaving, @EnableSpring Configured와 같은 애너테이션 기능 지원
  - 하이버네이트 4 지원
  - @Configuration 클래스와 빈 정의 프로파일에 대한 스프링 TestContext 프레임워크 지원
  - 생성자 주입 단순화를 위한 c: 네임스페이스
  - 서블릿 컨테이너에 대한 서블릿 3 코드 기반 구성 지원
  - persistence.xml 없이 JPA EntityManagerFactory를 부트스트랩(bootstrap)3하는 기능
  - 스프링 MVC에서 HTTP 세션 기반 리다이렉션(redirection)을 지원하는 Flash 및 Redirect Attributes 기능 추가
  - URI 템플릿 변수 확장
  - 스프링 MVC @RequestBody 컨트롤러 메서드 인수로 @Valid 애너테이션 사용
  - 스프링 MVC 컨트롤러 메서드 인수로 @RequestPart 애너테이션 사용
  
- 스프링 3.2.x 버전
  - 서블릿 3 기반 비동기 요청 처리 지원
  - 새로운 스프링 MVC 테스트 프레임워크 도입
  - 새로운 스프링 MVC 애너테이션 @ControllerAdvice과 @MatrixVariable 추가
  - RestTemplate과 @RequestBody에서 제네릭 형식 매개변수 지원
  - Jackson JSON 2 지원
  - 타일즈 3 지원
  - @RequestBody와 @RequestPart 인수에 Errors 인수를 전달받아 검증 에러를 처리
  - MVC 네임스페이스와 자바 Config 구성 옵션을 사용해 URL 패턴을 제외하는 기능
  - Joda Time없이 @DateTimeFormat 지원
  - 전 세계 날짜와 시간 형식 지원
  - 스코프(scoped)/프로토타입(prototyped) 빈의 생성에 대한 동시성 개선, 락(Lock) 최소화에 의한 프레임워크 전반의 동시성 향상
  - 그레이들(Gradle) 기반의 새로운 빌드 시스템 도입
  - 깃허브(GitHub)로 이전(https://github.com/SpringSource/spring-framework)
  - 프레임워크와 서브파티 의존성에 대한 자바 SE 7/OpenJDK 7의 지원 개선. CGLIB과 ASM이 스프링의 일부로 포함되고, AspectJ는 1.6 외에 1.7이 추가로 지원
  
- 스프링 4.0.x 버전: 스프링의 메이저 릴리스 버전이며, 처음으로 자바 8을 완전히 지원함. 이전 자바도 사용 가능하지만 최소 요구사항은 자바 SE 6이다.
  - 새로운 www.spring.io/guides 웹 사이트의 Getting Started(시작하기) 시리즈를 통해 스프링을 쉽게 시작할 수 있도록 개선
  - 이전의 스프링 3 버전에서 사용되지 않는 패키지와 메서드 제거
  - 자바 8 지원, 최소 자바 버전을 자바 6 업데이트 18 버전으로 변경
  - 자바 EE 6 이상은 스프링 프레임워크 4.0 버전으로 기준선 적용
  - 그루비(Groovy) 구문을 통해 빈 정의를 구성할 수 있는 그루비 빈 정의 DSL
  - 코어 컨테이너, 테스트, 일반 웹 개선
  - 웹소켓(WebSocket), SockJS 및 STOMP 메시징 지원
  
- 스프링 4.2.x 버전
  - 코어 개선(@AliasFor 도입과 이를 사용하는 기존 애너테이션 수정 등)
  - 하이버네이트 ORM 5.0에 대한 완벽한 지원
  - JMS 및 웹 개선
  - 웹소켓(WebSocket) 메시징 개선
  - 테스트 개선, 특히 @Rollback(false)를 대체하기 위해 @Commit을 도입했으며 스프링 프록시 뒷단의 기본 객체에 접근할 수 있도록 AopTestUtils 도입
  
- 스프링 4.3.x 버전
  - 프로그래밍 모델 개선
  - 코어 컨테이너(spring-core.jar에 ASM 5.1, CGLIB 3.2.4, Objenesis 2.4 포함)와 MVC 대폭적인 개선
  - Composed 애너테이션 추가
  - 스프링 TestContext 프레임워크는 JUnit 4.12 버전 이상 사용하도록 변경
  - 하이버네이트 ORM 5.2, 하이버네이트 Validator 5.3, 톰캣 8.5 & 9.0, Jackson 2.8 등 새로운 라이브러리 지원
  
- 스프링 5.0.x 버전: 스프링의 메이저 릴리스 버전이며, 프레임워크의 전체 코드를 자바 8을 기반으로 개발했고, 자바 9와도 완벽호환됨.
  - 포틀릿, 벨로시티(Velocity), JasperReports, XMLBeans, JDO, Guava, 타일즈 2, 하이버네이트 3에 대한 지원 중단
  - XML 구성 네임스페이스가 버전을 관리하지 않는 스키마로 변경. 버전별 선언은 아직까지 지원되지만 최신 XSD 스키마 여부에 대한 유효성 검증 수행
  - 자바 8의 기능을 최대한 활용하기 위해 전반적인 기능 개선
  - 자원 추상화에서 안전한 getFile 접근을 위한 isFile 지시자 제공
  - 스프링 제공 Filter 구현에서 완전한 서블릿 3.1 서명 지원
  - Protocol buffer = protobuf(직렬화 데이터 구조) 3.0 지원
  - JMS 2.0 +, JPA 2.1 + 지원
  - 스프링 MVC의 대안으로 리액티브 파운데이션 기반 프로젝트인 스프링 웹 플로우(Spring Web Flow) 도입. 스프링 웹 플로우는 완벽한 비동기 논블로킹(non-blocking) 방식으로 스레드마다 응답을 처리하는 전통적인 대형 스레드 풀 대신 이벤트 루프 실행 모델을 사용할 수 있도록 하며 Project Reactor4를 기반으로 합니다.
  - 웹 및 코어 모듈을 반응형 프로그래밍 모델5에 맞게 개선
  - 스프링 테스트 모듈에 많은 개선이 있었습니다. JUnit 5가 지원되며, @SpringJUnitConfig, @SpringJUnitWebConfig, @EnabledIf, @DisabledIf와 같은 Jupiter 프로그래밍 및 확장 모델을 지원하기 위해 새로운 애너테이션이 도입됐습니다.
  - 스프링 TestContext 프레임워크에서 병렬 테스트를 지원
  
- 스프링 5.1.x 버전
  - 자바 8 이상이 필요하며 공식적으로 자바 11을 지원합니다. 다음 LTS(long-term support)6 버전(자바 17) 릴리스까지 지원합니다.
  - Reactor Californium과 하이버네이트 ORM 5.3 지원
  - 코어 타입 및 애너테이션 분석 성능 향상
  - 기동 시간 향상과 힙 메모리 감소하기 위한 리플랙션 기능 최적화 수행
  - Reactor Netty 런타임 용도로 WebFlux HTTP/2 확장 지원