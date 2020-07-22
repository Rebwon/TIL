## Spring Security

애플리케이션에서 주의할 필요가 있는 보안 요소.

인증(Authentication), 권한 부여(Authorization), 크로스 사이트 스크립팅(XSS), SQL/NoSQL 인젝션 공격 예방을
기본적으로 애플리케이션 레벨에서 처리해줘야 함.

웹 애플리케이션을 어떻게 보호할 것인가?
- 사용자 인증하기
- 사용자 권한 부여하기
- 공격 예방하기

### 사용자 인증하기

사용자를 인증하는 일반적 프로세스는 사용자가 로그인 폼에 이메일 주소나 사용자 명을 비밀번호와 함께 제공하면서 시작된다.
그리고 애플리케이션은 이메일 주소나 사용자 명으로 DB에서 해당 사용자의 레코드를 찾는다. 그리고 DB에 저장된 비밀번호와 사용자가 입력한
비밀번호를 비교한다. 비밀번호 일치 여부에 따라 인증된 사용자로 간주하는 것을 결정한다. 그리고 보통은 인증된 사용자의 정보를
HTTP 세션에 저장해서 다음 요청부터는 사용자를 인식할 수 있다.

인증 프로세스에는 다양한 변형이 존재한다. 예를 들어, 사용자를 데이터베이스에서 찾아보는 대신, 애플리케이션이
액티브 디렉터리와 같은 Lightweight Directory Access Protocol 서버에서 찾아볼 수도 있다. 또는 사용자 데이터를 포함하는 XML 파일이나
메모리에 저장된 사용자 데이터를 활용할 수도 있다.

### 싱글 사인 온

여러 인증 프로세스의 변형 중 하나로 싱글 사인 온(Single Sign-On)이 있다. 싱글 사인 온은 SAML(Security
Assertion Markup Language) 또는 CAS(Central Authentication Service)와 같은 프로토콜을 사용한다. 싱글 사인 온을 사용하면
애플리케이션이 사용자를 아이덴티티 서버로 리다이렉트 시키고 인증을 수행하기 위해 아이덴티티 서버에 응답하고 나서 인증 결과를 애플리케이션에
알려준다. SAML을 활용하면 Service Provider인 애플리케이션은 인증 후에 SAML Assertion을 Identity Provider인 아이덴티티 서버로부터 받는다. SAML Assertion은
애플리케이션이 신뢰할 수 있는 사용자에 대한 정보를 포함한다. 보통 이름, 이메일 주소, 인증 제공자 측에서 활용하는 사용자 ID와 같은 기본 정보를 포함한다.
CAS를 사용하면 CAS 서버에서 인증이 성공해서 사용자가 애플리케이션으로 다시 리다이렉트되어 왔을 때 애플리케이션이 서비스 티켓을 받는다. 애플리케이션은 받은 티켓이 유효하고 신뢰할 수 있는지 확인하기 위해
티켓을 이용해 CAS 서버로 요청을 보낸다. 보다시피 싱글 사인 온 프로세스에서는 애플리케이션이 사용자의 신용에 대한 어떠한 정보도 가지지 않는다.

### 스프링 시큐리티 핵심 개념

스프링 시큐리티의 핵심 구성 요소는 Authentication, GrantedAuthority, SecurityContext, SecurityContextHolder이다.

스프링 시큐리티의 구성요소

- 공용 리소스에 접근하는 인증되지 않은 요청
- 보호 리소스에 접근하는 인증되지 않는 요청
- 인증된 요청
- 보호 리소스에 접근하는 인증된 요청
- 허가 받지 않은 리소스에 접근하는 인증된 요청

핵심 구성 요소의 매칭

- 접근 주체 -> Authentication(Principal 확장)
- 인증 -> AuthenticationManager
- 인가 -> Security Interceptor

Authentication의 용도
- 현재 접근 주체(Client)의 주체 정보를 담는 목적(Anonymous인지, User인지, Admin인지)
- 인증 요청할 때, 요청 정보를 담는 목적

SecurityContext의 용도
- Authentication을 보관(ThreadLocal에 보관함.)

내가 스프링 시큐리티를 막 공부했을 당시, Filter에 대한 개념이 부족했다.
FilterChain으로 체이닝 연결이 된 스프링 시큐리티 필터는 총 12개인데, 각각의 필터를 다 알 필요는 없지만
요구사항을 확장해야할 부분들의 필터의 동작 방식은 이해하는 것이 중요하고, 서블릿 필터 자체가 어떤식으로 동작하는지를 미리 알았다면 더 좋았을 것 같다.

정리하자면, 스프링 시큐리티의 필터들은 12개이고, 각각의 필터가 유기적으로 협력(filterChain.doFilter(req, res))하며 ThreadLocal을 구현한 SecurityContext에서 인증 정보를 확인하고,
그 다음 동작을 실행할 필터에게 전달하며 인증을 수행한다. 여기서 협력이란, 그 다음 필터를 호출하는 정도로 생각하자.

인증 Flow
- 사용자가 인증을 요청한다.
- SecurityFilterChain에 요청이 걸린다.
- 많은 Filter 중, UsernamePasswordAuthenticationFilter에 걸리게 되고, 이후에 AuthenticationManager를 사용해서 인증 정보를 생성하며
생성된 인증 정보를 SecurityContext에 담는다. 그리고 그 다음 Filter를 호출하며, SuccessHandler를 통해 인증 후처리를 담당한다.
- 만약, 인증에 실패할 경우 FailureHandler를 통해 인증 실패 후처리를 담당한다.
- 여기서 인증 성공, 실패 후처리는 각각 SuccessHandler, FailureHandler를 Default로 쓸 수 있지만, 확장하여 쓰는 것이 바람직하다.

### 스프링 시큐리티 필터

- WebAsyncManagerIntegrationFilter: 이 필터는 SecurityContext와 비동기 요청 처리를 위한 핵심 클래스인 스프링 웹의 WebAsyncManager 간의 통합을 제공한다.
- SecurityContextPersistenceFilter: 이 필터는 스프링 시큐리티가 정상적으로 동작하기 위해 필터 체인에 꼭 존재해야 하는 필터 중 하나다. 요청이 들어올 때 이 필터가 SecurityContextRepository에서 SecurityContext를 로딩해서
설정한다. SecurityContext가 로드되고 나면 SecurityContextHolder.getContext()로 SecurityContext에 접근이 가능하다. 반면에 여러 요청 간에 발생하는 SecurityContext는 SecurityContextRepository로 다시 업데이트된다. 이 필터는 또한 다른
사용자로부터 온 요청으로의 시큐리티 컨텍스트 유출 방지를 위해 SecurityContextHolder를 지운다.
- HeaderWriterFilter: 이 필터는 현재 요청에 대한 응답에 헤더를 추가한다. 예를 들어, 브라우저를 보호할 수 있도록 X-Frame-Options, X-XSS-Protection, X-Content-Type-Options와 같은 헤더를 추가한다.
- LogoutFilter: 이 필터는 인증된 사용자를 로그아웃 시킨다. 로그아웃을 처리하는 URL은 보통 /logout인데 HttpSecurity로 커스텀하게 설정이 가능하다.
- UsernamePasswordAuthenticationFilter: UsernamePasswordAuthenticationFilter는 인증이 시작되는 곳이다. 이 필터는 ProviderManager 인스턴스에 대한 참조를 가진다. ProviderManager는 AuthenticationManager 인터페이스의 구현체이다. ProviderManager는 실제 인증을 수행하기 위해 활용되는
AuthenticationProvider의 리스트를 가진다. 가장 널리 사용되는 인증 제공자는 DaoAuthenticationProvider다. DaoAuthenticationProvider는 요청으로 받은 비밀번호가 UserDetails에 있는 비밀번호와 일치하는지 검사한다. 일치하면 인증은 성공하고 그렇지 않으면 실패한다. 인증이 성공하면 UsernamePasswordAuthenticationFilter는 SecurityContext를 업데이트한다. 
그리고 사용자를 인증된 것으로 여긴다. AuthenticationSuccessHandler가 요청에 대한 주도권을 가지며 사용자는 기본 성공 URL로 리다이렉트된다. 인증이 실패하면 SecurityContextHolder는 지워지며 AuthenticationFailureHandler가 요청에 대한 주도권을 가진다. 기본으로 사용자는 /login?failed 페이지로 리다이렉트된다. UsernamePasswordAuthenticationFilter는 어떤 요청에도 인증 프로세스를 시작하지 않는다. 
UsernamePasswordAuthenticationFilter는 HTTP POST 메소드로 /login 경로로 전송된 요청을 위해서만 인증 프로세스를 시작한다.
- RequestCacheAwareFilter:이 필터는 이전에 캐시된 요청을 복원한다. 이전에 캐시된 요청은 대게 요청이 실패할 때 ExceptionTranslationFilter에 의해 저장된다. 예를 들어, 하나의 인증된 요청이 보호되는 리소스에 접근하기 위해 전송됐을 때 스프링 시큐리티는 이 요청을 차단한다. 그리고 사용자를 로그인 페이지로 보낸다. 인증이 성공한 후에 사용자는 다시 리다이렉트돼서 보호 리소스로 온다. 
스프링 시큐리티는 캐시된 요청을 확인해서 인증 후 사용자가 어디로 가야 하는지 알 수 있다.
- SecurityContextHolderAwareRequestFilter: 이 필터는 다음과 같은 서블릿 API 보안 메소드를 구현한 래퍼 (wrapper)를 활용하는 요청에 대한 책임이 있다.
  - HttpServletRequest.authenticate(HttpServletResponse)
  - HttpServletRequest.login(String, String)
  - HttpServletRequest.logout()
- AnonymousAuthenticationFilter: 이 필터는 Security Context에 어떤 Authentication이 존재하지 않으면 SecurityContext를 AnonymousAuthenticationToken으로 업데이트한다. 이 필터는 익명 사용자에 대해 실제로 인증을 수행하지는 않는다. 이 필터는 요청이 익명 사용자로부터 온 것임을 확실히 하기 위해 플레이홀더 인증 (placeholder authentication)을 SecurityContext에 저장한다.
- SessionManagementFilter: 이 필터는 세션 고정 보호(session fixation protection)를 활성화하는 책임과 동시 세션(concurrent sessions)을 컨트롤하는 책임이 있다. 동시 세션은 ConcurrentSessionFilter가 필터 체인에 있어야 한다. HttpSecurity가 제공하는 다음 설정으로 동시 세션 관리를 활성화할 수 있다. 기본적으로 세션 수에 제한은 없다.
  - http sessionManagement().maximumSessions(n);
- ExceptionTranslationFilter: 필터 체인의 마지막에서 두 번째 필터다. 이름이 말해주듯이, 이 필터는 스프링 시큐리티의 예외를 해석하는 책임이 있다. AuthenticationException 예외가 잡히면 이 필터는 인증 엔드 포인트를 시작한다. AccessDeniedException 예외가 잡히고 사용자가 익명일지라도 이 필터는 인증 엔드 포인트를 시작한다. 사용자가 익명이 아니면 AccessDeniedHandler는 기본적으로 HTTP 403 응답을 전송한다.
- FilterSecurityInterceptor: 필터 체인의 마지막 필터는 FilterSecurityInterceptor다. FilterSecurityInterceptor는 AccessDecisionManager에 대한 참조를 가진다. 이 필터는 SecurityContext를 가져온다. 그다음 허용된 요청인지 결정하는 작업을 AccessDecisionManager에게 위임한다. 그리고 요청이 거부되면 예외를 발생시킨다. 요청이 허용되면 요청은 대응하는 Controller에 도달한다. 
AccessDecisionManager가 내리는 결정은 투표에 기반한다. 여기서 결정은 요청 단위다. 이것은 스프링 시큐리티가 요청의 경로를 HttpSecurity의 설정과 비교하여 확인하고 SecurityContext에 있는 부여된 권한이 접근을 허용하기에 충분한지 확인한다는 의미다.

### 사용자 정보 가져오는 방법

- HttpServletRequest.getUserPrincipal()으로 현재 사용자 정보를 가져올 수 있다. 이 경우 스프링이 HttpServletRequest를 API 핸들러 메서드로 주입해야 한다.
- SecurityContextHolder로 SecurityContextHolder.getContext().getAuthentication()처럼 직접 가져올 수도 있다.
- 스프링 시큐리티의 AuthenticationPrincipal이 적용된 어노테이션을 생성할 수 있다. 그리고 스프링에 UserDetails 구현체를 API 핸들러 메소드에 주입하도록 요청할 수 있다.