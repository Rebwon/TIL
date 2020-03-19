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