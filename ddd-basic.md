## DDD 구현 기초 정리

출처 : https://www.slideshare.net/madvirus/ddd-final

### 클래스 : 객체의 정의

- 기능(메세징)을 public 메서드로 정의
  - 데이터는 외부에 드러내지 않고 기능 정의만 제공.
    - getter/setter 메서드 최소화

- 다른 객체를 사용하는 방법
  - 필드를 이용해서 다른 객체를 레퍼런스로 참조
    - 연관(Association)이라고 표현
  - 메서드에서 필요한 시점에 객체를 생성해서 사용
  
객체 간 연관의 예
```java
public class Employee{
    private String name;
    private Organization organization;
    
    public void setOrganization(Organization org) {
        this.organization = org;
    }   
}

public class Organization{
    private String name;
    private List<Employee> employees;
    
    public void add(Employee emp){
        this.employees.add(emp);
        emp.setOrganization(this);
    }
}

Organization org = new Organization();

Employee e1 = new Employee();
Employee e2 = new Employee();

org.add(e1);
org.add(e2);
```

### 연관의 방향성

- 방향성의 종류
  - 단방향: 두 객체 중 한 객체만 레퍼런스 가짐
  - 양방향: 두 객체가 서로에 대한 레퍼런스 가짐

### 연관의 종류

- ManyToOne
  - 예, 다수의 Employee가 한 Organization과 연관
- OneToMany
  - 예, 한 Organization이 다수의 Employee와 연관
  - 콜렉션으로 구현: Set, List, Map
- OneToOne
  - 예, 한 Organization이 한 Rule과 연관
- ManyToMany
  - 콜렉션
  
### 모든 객체가 메모리 있다면... (1)

- 연관된 객체들을 이용해서 기능을 구현
- 예1) Rule의 교체: 연관된 객체 교환으로 구현

```java
Organization org = ..; // 어딘가에서 구함(JPA)
Rule newRule = new Rule();
org.changeRule(newRule);

// Organization의 메서드
public void changeRule(Rule newRule){
    this.rule = newRule;
}
```

- 예2) 팀 이동: 양방향 연관을 처리
```java
Organization org = ..; // 어딘가에서 구함(JPA)
Employee emp1 = ..; // 어딘가에서 구함(JPA)
emp1.trasferTo(org);

// Employee의 메서드
public void transferTo(Organization org){
    Organization oldOrg = this.organization;
    this.organization = org;
    oldOrg.remove(this);
}

// Organization의 메서드
public void remove(Employee e){
    employees.remove(e);
}
```

### 모든 객체가 메모리 있다면.. (2)

- 연관된 객체들을 이용해서 기능을 구현
- 예3) Rule의 확인: 연관된 객체를 구해 확인

```java
Organization org = ..; // 어딘가에서 구함(JPA)
Rule rule = org.getRule();
if(rule != null){
    rule.check(someData);
}

// Organization의 메서드
public Rule getRule(){
    return this.rule;
}

// Rule의 메서드
public void check(Data data) {...}
```

- 예4) Rule의 확인: 내부적으로 위임
```java
Organization org = ..; // 어딘가에서 구함(JPA)
org.checkRule(someData);

// Organization의 메서드
public void checkRule(Data data){
    if(rule == null){
        return;
    }   
    rule.check(data);
}
```

### 모든 객체가 메모리 있다면.. (3)

- 객체를 메모리에 보관/검색 위한 방법 필요
  - 객체마다 고유 식별값 필요
    - 메모리 상의 레퍼런스 이용은 한계
    - 고유 식별 값을 갖는 객체를 Entity라 함.
  - 객체 보관하고 제공해주는 기능 필요
    - DDD에서는 이를 Repository로 추상화
    
### 객체의 라이프사이클

- 객체 생성 - 사용 - 소멸
  - 동일 라이프사이클을 갖는 객체들의 군집 존재
  - 동일 라이프사이클 == 한 트랜잭션 단위

### 도메인 모델의 기본 구성 요소

- Entity
- Value
- Aggregate
- Repository
- Service

### 도메인 모델의 기본 구성 요소, Entity

- 주요 특징
  - 모델을 표현
  - 고유의 식별 값을 가짐
  - 모델과 관련된 비즈니스 로직을 수행
  - 자신만의 라이프 사이클을 가짐
- 예, 평가 시스템에서의 Entity
  - Employee
  - Organization
  
### 도메인 모델의 기본 구성 요소, Value

- 고유의 키 값을 갖지 않음
- 데이터를 표현하기 위한 용도로 주로 사용
  - 개념적으로 완전한 데이터 집합
  - (주로) 불변임
  - 자신에게 알맞은 로직을 제공
  - Entity의 속성으로 사용
    - 자신의 라이프사이클을 갖지 않음
    - 엔티티를 따름
- 예
  - Address(주소)
    - 데이터: 우편번호, 주소1, 주소2
  - Money(돈)
    - 데이터: 금액, 화폐 단위
    - 로직: 더하기, 곱하기 등

### 도메인 모델의 기본 구성 요소, Aggregate

- Aggregate
  - 관련된 객체들의 묶음
- Aggregate의 특징
  - 데이터 변경 시 한 단위로 처리됨
    - 비슷한 또는 거의 유사한 라이플사이클을 갖는 객체들
      - 특히, 삭제 시 함께 삭제됨
  - Aggregate은 경계를 가짐
    - 기본적으로, 한 Aggregate에 속한 객체는 다른 Aggregate에 속하지 않음.

- Aggregate의 필요성
  - 많은 도메인 모델을 가능한 간단하고 이해가능한 수준으로 만들 필요
  - 연관의 개수를 줄임으로써 복잡도 감소
  
### 도메인 모델의 기본 구성 요소, Aggregate 루트

- Aggregate은 루트를 가짐
- Aggregate 루트 역할
  - Aggregate의 내부 객체들을 관리
  - Aggregate 외부에서 접근할 수 있는 유일한 객체
    - Aggregate의 내부 Entity/Value는 루트를 통해 접근
  - Aggregate에 대한 알맞은 기능을 수행
    - 예) 주문 관련 Aggregate 루트인 Order의 기능
      - 주문 취소, 배송지 주소 변경, 주문 상품 변경
    - 내부 객체들의 연관을 탐색해서 기능 구현
    
### 레이어 아키텍처

엔터프라이즈 애플리케이션을 설계할 때, 주로 우리는 레이어 아키텍처를 많이 사용한다.
레이어 아키텍처의 전형적인 영역이 '표현', '응용', '도메인', '인프라스트럭처'의 네 영역이다.

도메인 주도 설계에서 표현 영역을 통해 사용자의 요청을 전달받는 응용 영역은 시스템이 사용자에게 제공해야할 기능을 구현한다.

응용 영역은 기능을 구현하기 위해 도메인 영역의 도메인 모델을 사용한다. 주문 취소 기능을 제공하는 응용 서비스를 예로 들면
다음과 같이 주문 도메인 모델을 사용해서 기능을 구현한다.

```java
public class CancelOrderService{
    @Transactioanl
    public void cancelOrder(Long orderId) {
        Order order = orderRepository.findById(orderId)
                .orElseThrow(() -> new IllegalArgumentException("No Order"));
        order.cancel();
    }
}
```

응용 서비스는 로직을 직접 수행하기보다는 도메인 모델에 로직 수행을 *위임*한다. 위 코드도 주문 취소 로직을 직접 구현하지 않고
Order 객체에 취소 처리를 위임하고 있다.

도메인 영역은 도메인 모델을 구현한다. 도메인 모델은 도메인의 핵심 *로직*을 구현한다. 주문 도메인의 경우 '배송지 변경', '결제 완료', '주문 총액 계산'과 같은
핵심 로직을 도메인 모델에서 구현한다.

인프라스트럭처 영역은 구현 기술에 대한 것을 다룬다. 이 영역은 RDBMS 연동을 처리하고, 메시징 큐에 메시지를 전송하거나 수신하는 기능을 구현하고, SMTP를 통해 메일 발송
기능을 구현하거나 REST API 호출하는 것도 처리한다.

인프라스트럭처 영역은 논리적인 개념을 표현하기보다는 실제 구현을 다룬다.

레이어 아키텍처에서는 상위 계층에서 하위 계층으로의 의존만 존재하고, 하위 계층은 상위 계층에 의존하지 않는다.
예를 들어, 표현 계층은 응용 계층에 의존하고 응용 계층이 도메인 계층에 의존하지만, 반대로 인프라스트럭처 계층이 도메인에 의존하거나 도메인이 응용 계층에 의존하지 않는다.

레이어 아키텍처를 엄격하게 적용하면 상위 계층은 바로 아래의 계층에만 의존을 가져야 하지만 구현의 편리함을 위해 계층 구조를 유연하게 적용한다. 예를 들어, 응용 계층은
바로 아래 계층인 도메인 계층에 의존하지만 외부 시스템과의 연동을 위해 더 아래 계층인 인프라스트럭처 계층에 의존하기도 한다.

의존성의 방향이 위에서 아래로 흐르는 것이 맞지만, 이럴 경우 상위계층인(표현, 응용, 도메인) 계층이 인프라스트럭처 계층에 종속되게 된다.

모든 계층이 인프라스트럭처 계층에 의존하면 '테스트 어려움'과 '기능 확장의 어려움'이라는 두 가지 문제가 발생하는 것을 알았다.

이러한 문제를 해결하는 것은 DIP를 적용하는 것이다.

### DIP

고수준 모듈이 제대로 동작하려면 저수준 모듈을 사용해야 한다. 그런데, 고수준 모듈이 저수준 모듈을 사용하면 앞서 계층 구조 아키텍처에서 언급했던 두 가지 문제(구현 변경과 테스트가 어려움)가 발생한다.

DIP는 이 문제를 해결하기 위해 저수준 모듈이 고수준 모듈에 의존하도록 바꾼다. 고수준 모듈을 구현하려면 저수준 모듈을 사용해야 하는데, 반대로 저수준 모듈이 고수준 모듈에
의존하도록 하려면 어떻게 해야 할까? 비밀은 추상화한 인터페이스에 있다.

DIP를 적용할 때 하위 기능을 추상화한 인터페이스는 고수준 모듈 관점에서 도출한다. 

### 모듈 구성

기본적으로 레이어 아키텍처에 맞게 패키지를 구성한다.

도메인이 크면 각 도메인 안에 레이어 아키텍처에 맞게 패키지를 구성한다.

### 에그리거트 별 트랜잭션 범위

에그리거트 별로 별도의 트랜잭션으로 관리하되, 한 에그리거트가 다른 에그리거트를 수정해서는 안된다. 그렇게 하게되면 모듈간 의존성을 높이기 때문에
권장하지 않는다. 대신, 응용 서비스 영역에서 서로 다른 에그리거트 두개를 수정하는 것이 바람직하다.

한 트래잭션에서 한 개의 에그리거트를 변경하는 것을 권장하지만, 다음의 경우에는 한 트랜잭션에서 두 개 이상의 에그리거트를 변경하는 것을 고려할 수 있다.

- 팀 표준: 팀이나 조직의 표준에 따라 사용자 유스케이스와 관련된 응용 서비스의 기능을 한 트랜잭션으로 실행해야 하는 경우가 있다. DB가 다른 경우 글로벌 트랜잭션을 반드시
사용하도록 규칙을 정하는 곳도 있다.
- 기술 제약: 한 트랜잭션에서 두 개 이상의 에그리거트를 수정하는 대신 도메인 이벤트와 비동기를 사용하는 방식을 사용하는데, 기술적으로 이벤트 방식을 도입할 수 없는 경우 한
트랜잭션에서 다수의 에그리거트를 수정해서 일관성을 처리해야한다.
- UI 구현의 편리: 운영자의 편리함을 위해 주문 목록 화면에서 여러 주문의 상태를 한 번에 변경하고 싶을 것이다. 이 경우 한 트랜잭션에서 여러 주문 에그리거트의 상태를 변경할 수 있을 것이다.

### 에그리거트와 리포지터리

에그리거트는 개념상 완전한 한 개의 도메인 모델을 표현하므로, 객체의 영속성을 처리하는 리포지터리는 에그리거트 단위로 존재한다. 

