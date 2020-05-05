## GenericPropertyEditor를 만들려면..

출처 : https://whiteship.tistory.com/1606

- PropertyEditor가 뭘까?

https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyEditor.html

위 문서를 참조한 결과 PropertyEditor는 사용자가 입력한 값을 적절한 자바빈 스펙의 데이터로 변환해주기 위해
필요한 인터페이스이다.

쉽게 말해 사용자가 입력한 값을 애플리케이션 도메인 객체에 동적으로 할당해주는 기능이다.

- PropertyEditor를 왜 쓸까?

사용자가 form에 데이터를 입력해줄때 입력받는 데이터는 보통 문자열이다. 사용자가 입력한 문자열을 자바의 다양한 타입(primitive, reference)으로
변환해주기 위해서 사용한다.

- 주의 사항

PropertyEditor가 가지고 있는 값은 서로 다른 쓰레드에 공유가 될 수 있다. 따라서 stateful하기 때문에 쓰레드 safe하지 않는다. 즉, 여러 쓰레드에 공유해서 사용하면 안된다(Bean으로 등록해서 사용하면 안된다) Bean으로 등록해서 사용하면, 1번 회원이 2번 회원 정보를 수정하는 등 심각한 문제가 생길 수 있다.

- propertyEditor 사용 예제

스프링에서 @InitBinder를 사용해서 등록한다.

https://github.com/eugenp/tutorials/tree/master/spring-boot-modules/spring-boot-data/src/main/java/com/baeldung/propertyeditor

아래는 백기선님이 직접 작성한 Generic Property Editor 코드이다.

```java
/**
 * Generic PropertyEditor uses dao to load entity by id.
 * @author Whiteship
 * @param <T> Entity class type
 * @param <D> GenericDao type
 */
public abstract class AbstractGenericPropertyEditor<T, D extends GenericDao<T, ?>> extends PropertyEditorSupport {

    Log logger = LogFactory.getLog(getClass());

    protected Class<T> entityClass;
    protected Class<D> daoClass;
    protected D dao;

    public AbstractGenericPropertyEditor(Class<T> entityClass, Class<D> daoClass,
            ApplicationContext applicationContext) {
        this.entityClass = entityClass;
        this.daoClass = daoClass;
        dao = (D) ApplicationContextUtils.getBeanByType(applicationContext, daoClass);
    }

    /** Entity object -> Entity's id. */
    @SuppressWarnings("unchecked")
    public String getAsText() {
        T entity = (T) this.getValue();
        logger.debug("entity = " + entity);
        if (entity == null)
            return "";
        else
            return String.valueOf(ReflectionUtils.callMethod(entityClass, entity, "getId"));
    }

    /** Entity's id -> Entity object by dao. */
    @Override
    public void setAsText(String id) throws IllegalArgumentException {
        logger.debug("text = " + id);
        if (id == null || id.trim().length() == 0)
            setValue(null);
        else
            setValue(dao.get(Integer.parseInt(id)));
    }

}
```

### 논외

property editor와 관련해서 백기선님의 글을 정독하던 중, OSAF라는 프레임워크의 존재를 알았다.

아래는 그 프레임워크의 주소이다.

https://github.com/keesun/OSAF

이 안에 Generic을 활용한 예제들을 찾아볼 수 있었다. 좋은 공부 자료가 될 것 같다.