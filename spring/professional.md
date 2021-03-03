## VMWare Spring Professional Q & A

### Container, Dependency and IOC

- DI란 무엇이며, 이를 사용할 때의 이점은 무엇입니까?
  - DI(Dependency Injection)이란 한국말로 의존관계 주입이라고 합니다. DI는 오브젝트 레퍼런스를 외부로부터 제공(주입)받고 이를 통해 여타 오브젝트와 다이내믹하게 의존관계가 만들어지는 것입니다. DI를 사용할 때의 이점은 유연하고 변경에 용이하다는 것입니다.
- 인터페이스란 무엇이며, Java에서 인터페이스를 사용할 때의 이점은 무엇입니까?
  - 인터페이스란 추상화된 어떠한 동작을 정의한 설계도다. 자바에서 인터페이스를 사용하면 의존 관계를 적절하게 분리할 수 있고, 확장에는 유연하고 변경에는 닫혀있는 구조를 만드는 이점이 생깁니다.  
- ApplicationContext란 무엇입니까?
  - 스프링의 빈 생성과 의존관계 설정 같은 제어를 담당하며, 그 밖에 다양한 기능을 제공하는 추상화된 인터페이스이다.
- ApplicationContext의 새 인스턴스를 만드는 방법은 무엇입니까?
  - @Configuration을 사용하여 사용할 빈을 정의하고 그 정의한 @Configuration이 붙어있는 클래스를 new 키워드를 통해 ApplicationContext의 생성자로 넘겨준다.
- ApplicationContext에서 Spring Bean의 라이프사이클을 설명할 수 있습니까?
  - @Configuration을 통해 정의된 Configuration 정보를 생성자를 통해 얻어서 @Bean이 붙은 메소드의 이름을 가져와서 빈 목록을 만듭니다. 이때 만들어지는 빈은 싱글톤 스코프이며, 강제로 제거하거나 하지 않는 이상 스프링 컨테이너가 존재하는 동안 같이 존재합니다. 클라이언트에서 getBean()을 호출하면 자신(애플리케이션컨텍스트)의 빈 목록에서 요청한 이름이 있는지 찾고, 있다면 빈을 생성하는 메소드를 호출해서 오브젝트를 생성시킨 후 클라이언트에 넘겨줍니다.
- 통합 테스트에서 ApplicationContext를 어떻게 작성할 예정입니까?
- 애플리케이션 컨텍스트를 닫기 위해 선호하는 방법은 무엇입니까? Spring Boot가 이 기능을 수행합니까?
  - ApplicationContext를 ConfigurableApplicationContext로 다운 캐스팅 한 후 .close() 를 실행합니다. ConfigurableApplicationContext는 Closeable 인터페이스를 구현하였기 때문에 .close() 메소드가 제공됩니다. 또한 SpringBoot에 메인 메소드에서 사용하는 클래스인 SpringApplication.run()에서 리턴값으로 제공하는 것이 바로 ConfigurableApplicationContext이기 때문에 SpringBoot 역시 해당 기능을 다음과 같이 수행할 수 있습니다. SpringApplication.run(SampleApplication.class, args).close()
- 다음을 설명하세요.
  - Java 구성을 사용한 DI는 무엇입니까?
  - @Autowired를 사용하여 DI를 수행합니까?
  - Component Scanning, Stereotypes는 무엇입니까?
  - Spring Bean의 범위와 기본 범위는 무엇입니까?
    - 스프링 빈의 범위는 싱글톤, 프로토타입, 세션, request 이며, 기본 범위는 싱글톤입니다.
- 빈은 기본적으로 lazy 또는 eager로 인스턴스화 됩니까? 어떻게 이런 행동을 바꿀까요?
- @PropertySource는 어디에 사용하며 무엇입니까?
- BeanFactoryPostProcessor는 무엇이며 어디에 사용하고 언제 호출됩니까?
  - 자신만의 BeanFactoryPostProcessor를 만들 때 정적 @Bean 방법을 정의하는 이유는 무엇입니까?
  - PropertySourcePlaceholderConfigurer는 무엇에 사용됩니까?
- BeanPostProcessor란 무엇이며 BeanFactoryPostProcessor와 어떻게 다릅니까?
그들은 무엇을 합니까? 그들은 언제 부릅니까?
  - 초기화 방법은 무엇이며, 어떻게 Spring Bean에 선언됩니까?
  - 파괴 방법이란 무엇이며, 어떻게 선언되며, 언제 부릅니까?
  - @PostConstruction 및 @PreDestroy와 같은 JSR-250 주석을 활성화하는 방법을 고려해 보시기 바랍니다.
그들은 언제/어떻게 부릅니까?
  - 스프링 빈의 초기화 또는 제거 방법을 어떻게 정의할 수 있습니까?
- 구성 요소 스캐닝은 어떤 기능을 합니까?
- @Qualifier는 어떻게 @Autowired의 사용을 보완합니까?
- 프록시 오브젝트란 무엇이며 스프링이 생성할 수 있는 두 가지 다른 프록시 유형은 무엇입니까?
  - 이러한 프록시(유형별)의 제한은 무엇입니까?
  - 프록시 객체의 힘은 무엇이며 단점은 어디에 있습니까?
- @Bean의 기능은 무엇입니까?
- @Bean만 사용하는 경우 기본 빈 ID는 무엇입니까? 이걸 어떻게 무시할 수 있죠?
- @Configuration을 final 클래스에 달 수 없는 이유는 무엇입니까?
  - @Configuration이 달린 클래스는 싱글톤 빈을 어떻게 지원합니까?
  - @Bean 방법 역시 final이 될 수 없는 이유는 무엇입니까?
- profile을 구성하는 방법은 무엇입니까? 유용한 사례는 무엇입니까?
- @Bean을 @Profile과 함께 사용할 수 있습니까?
- @Profile과 함께 @Component를 사용할 수 있습니까?
- 얼마나 많은 profile을 가질 수 있나요?
- 어떻게 스칼라/리터럴 값을 스프링 빈에 주입합니까?
  - @Value는 무엇입니까?
- SPEL은 무엇입니까?
- 스프링의 환경 추상화란 무엇입니까?
- SPEL을 사용하여 무엇을 참조할 수 있습니까?
- @Value 식에서 $와 #의 차이는 무엇입니까?

### Aspect-Oriented Programming (AOP)

- AOP의 개념은 무엇입니까? 어떤 문제를 해결합니까? 크로스 커팅이란 무엇입니까?
  - 일반적인 크로스 커팅 문제 세 가지를 설명합니다.
  - 크로스 커팅 문제를 AOP를 통해 해결하지 못할 경우 발생하는 두 가지 문제는 무엇입니까?
- pointcut, join point, advice, aspect, weaving은 무엇입니까?
- Spring이 크로스 커팅 문제를 어떻게 해결(실행)합니까?
- 두 가지 프록시 유형의 제한 사항은 무엇입니까?
  - 스프링의 AOP를 사용하여 스프링 빈 방법을 대체해야할 때 가시성은 무엇입니까?
- 스프링에서 지원하는 advice 유형은 몇 가지입니까?
  - 이러한 용도는 무엇에 사용됩니까?
  - 예외를 포착하고 싶을 때 사용할 수 있는 두 가지 advice는 무엇입니까?
- JoinPoint 인수는 무엇에 사용됩니까?
- ProcessingJoinPoint란 무엇입니까? 어떤 종류의 advice와 함께 사용됩니까?