## DTO에서는 Setter가 필요없을까?

-  Get 메소드를 통해 반환하는 DTO에서는 Setter 메소드가 필요하다.
   - 왜? -> Get 요청의 경우 JSON 데이터가 아닌 Query Parameter를 보내기 때문에, Spring에서는 WebDataBinder를 사용하게 된다.
   WebDataBinder의 경우 기본값으로 값을 할당하는 방식이 Java Bean 방식이라고 한다. 그렇기 때문에 별다른 설정이 없다면 Setter를 사용해야 한다.

그렇다면, Get에서도 Setter를 사용할 방법이 없을까?

WebDataBinder를 빈으로 등록해서 기본 값인 initBeanPropertyAccess가 아닌 initDirectFieldAccess를 선언함으로써, 
Setter가 아닌 Field에 직접 접근하는 방법이다. 

사용하는 방법은 모든 Controller에서 사용이 가능하도록, ControllerAdvice를 선언하여 WebDataBinder를 빈으로 등록 후 기본설정을 변경하는 것이다.

```java
@Slf4j
@ControllerAdvice
public class WebControllerAdvice {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        binder.initDirectFieldAccess();
    }
}
```

출처 : https://jojoldu.tistory.com/407