## AttributeConverter 사용시 주의사항

AttributeConverter를 사용하는 이유는, 커스텀한 값 타입의 객체를 데이터베이스의 컬럼 타입과 매핑시키기 위함이다.

AttributeConverter를 사용할 때, 엔티티 클래스의 필드 변수에 컨버팅해줘도 되고, @Convert(autoApply = true)를 통해
전체 패키지에서 사용 가능하게 할 수 있다.

여기서 이제 중요한 점은, autoApply를 true로 변경해서 전체 패키지에서 사용할 경우 해당 컬럼에 값이 들어가지 않거나,
기본값이 주어지지 않으면 NullPointException을 발생시킨다.

따라서, AttributeConverter를 사용한 커스텀한 값 타입의 객체는 무조건 기본값을 설정하거나, 데이터베이스에 INSERT 할 때
넣어주도록 하자.