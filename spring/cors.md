## Spring에서 Cors 설정

만약이란 없지만, CORS를 세세하게 설정할 필요가 없는 내부 시스템에 경우 WebMvcConfigurer를 확장하여 addCorsMappings를 설정해주면
좋다.

아래와 같이 말이다.
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
	private final long MAX_AGE_SECS = 3600;

	@Override
	public void addCorsMappings(CorsRegistry registry) {
		registry.addMapping("/**")
			.allowedOrigins("*")
			.allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS")
			.allowedHeaders("*")
			.allowCredentials(true)
			.maxAge(MAX_AGE_SECS);
	}
}
```

하지만, 모든 앱에는 보안적인 요구사항이 필수 불가결하다. 따라서 Spring Security를 쓰게될 것이고, 그렇다면 까다로운 보안적 요구사항을
만족하려면, CustomFilter와 CorsConfigurationSource를 통해 설정해야 한다.

WebMvcConfigurer를 통한 설정을 해도 Security에서는 기존 설정을 똑똑하게 알고 사용한다. 

추가로, WebMvcConfigurer를 사용하지 않고 확장할 경우, requestMatchers(CorsUtils::isPreFlightRequest).permitAll() 이 부분을
Security Configuration에 추가해야 한다. 그렇지 않을 경우 Social Login 등의 OPTION 요청을 받을 때 403에러가 발생한다.