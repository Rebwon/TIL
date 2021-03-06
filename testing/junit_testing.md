## 좋은 테스트의 First 속성

단위 테스트는 주의 깊게 사용했을 때 많은 이점을 얻을 수 있다. 하지만 테스트 또한 내가 작성하고 유지 보수해야 하는 코드이다. 따라서 테스트에 다음의 문제점이 있다면 매우 피곤해진다.

- 테스트를 사용하는 사람에게 어떠한 정보도 주지 못하는 테스트
- 산발적으로 실패하는 테스트
- 어떤 가치도 증명하지 못하는 테스트
- 실행하는 데 오래 걸리는 테스트
- 코드를 충분히 커버하지 못하는 테스트
- 구현과 강하게 결합되어 있는 테스트, 따라서 작은 변화에도 많은 테스트가 깨진다.
- 수많은 설정 고리로 점프하는 난해한 테스트

다음의 First 원칙을 따르면 단위 테스트를 작성하다가 빠지는 위험을 피할 수 있다.

- Fast 빠르게
- Isolated 고립된
- Repeatable 반복 가능한
- Self-validating 스스로 검증 가능한
- Timely 적시의

### Fast

테스트를 빠르게 유지하라. 설계를 깨끗하게 하면 빠르게 유지할 수 있다. 가장 먼저 느린 테스트에 대한 의존성을 줄이자. 모든 테스트 코드가 데이터베이스를 호출한다면 전체 테스트 또한 느려진다. 의존성을 최소화하는 것 역시 좋은 설계의 목표다.

### Isolated

좋은 단위 테스트는 검증하려는 작은 양의 코드에 집중한다. 직접적 혹은 간접적으로 테스트 코드와 상호 작용하는 코드가 많을수록 문제가 발생할 소지가 늘어난다.

데이터 의존성은 많은 문제를 만들어 낸다. 
- 데이터베이스에 올바른 데이터가 있는지 확인해야함.
- 데이터 소스를 공유한다면 테스트를 깨뜨리는 외부 변화도 걱정해야함.
- 다른 개발자들이 동시에 테스트 코드를 실행할 수 있다는 것도 생각해야함.

외부 저장소와 상호 작용하게 되면, 테스트가 가용성 혹은 접근성 이슈로 실패할 가능성이 증가한다.

테스트 코드는 어떠한 순서나 시간에 관계없이 실행되어야 한다.

테스트 코드가 하나 이상의 이유로 깨진다면 테스트를 분할하는 것도 고려해 보아야 한다.

### Repeatable

반복 가능한 테스트는 실행할 때마다 결과가 같아야 한다. 따라서 반복 가능한 테스트를 만들려면 직접 통제할 수 없는 외부 환경에 있는 항목들과 격리시켜야 한다.

불가피하게 통제할 수 없는 요소(랜덤, 시간 등)와 상호 작용해야한다면, 테스트 대상 코드의 나머지를 격리하고 통제할 수 없는 요소에 목 객체를 사용하여야 한다.

### Self-validating

테스트는 스스로 검증 가능할 뿐만 아니라 준비할 수도 있어야 한다. 테스트를 실행하기 전에 수동으로 준비 단계를 만드는 어리석은 짓은 하면 안된다. 테스트에 필요한 어떤 설정 단계든 자동화를 해야 한다. 테스트를 실행하는 데 외부 설정이 필요하다면 FIRST 중 고립성을 위반한 것이다.

### Timely

언제라도 단위 테스트를 작성할 수 있지만, 적시에 꼭 필요한 순간에 단위 테스트에 집중해야한다.

## Right-BICEP

Right-BICEP는 무엇을 테스트할지에 대해 쉽게 선별하게 한다.
- Right : 결과가 옳은가?
- B : 경계 조건(Boundary condition)은 맞는가?
- I : 역 관계(Inverse relationship)를 검사할 수 있는가?
- C : 다른 수단을 활용하여 교차 검사(Cross-Check)할 수 있는가?
- E : 오류 조건(Error conditions)을 강제로 일어나게 할 수 있는가?
- P : 성능 조건(Performance conditions)은 기준에 부합하는가?

생각해야 할 경계 조건
- 모호하고 일관성 없는 입력 값. 예를 들어 특수 문자가 포함된 파일 이름
- 잘못된 양식의 데이터. 예를 들어 최상위 도메인이 빠진 이메일 주소
- 수치적 오버플로를 일으키는 계산
- 비거나 빠진 값. 예를 들어 0, 0.0, "" 혹은 null
- 이성적인 기댓값을 훨씬 벗어나는 값. 예를 들어 150세의 나이
- 교실의 당번표처럼 중복을 허용해서는 안 되는 목록에 중복 값이 있는 경우
- 정렬이 안된 정렬 리스트 혹은 그 반대. 정렬 알고리즘에 이미 정렬된 입력 값을 넣는 경우나, 퀵 정렬 알고리즘에 역순 데이터를 넣는 경우
- 시간 순이 맞지 않는 경우. 예를 들어 HTTP 서버가 OPTIONS 메서드의 결과를 POST 메서드보다 먼저 봔한해야 하지만 그 후에 반환하는 경우

코드를 테스트하기 위해 도입할 수 있는 오류의 종류 혹은 다른 환경적 제약은 아래와 같다.
- 메모리가 가득 찰 때
- 디스크 공간이 가득 찰 때
- 서버와 클라이언트 간 시간이 달라서 발생하는 문제들
- 네트워크 가용성 및 오류들
- 시스템 로드
- 제한된 색상 팔레트
- 매우 높거나 낮은 비디오 해상도

0, 1, n 경계 조건
- 모든 테스트는 0, 1, n개의 경계조건이 있다. n은 비즈니스 요구사항에 따라 달라질 수 있다.

## 내부 데이터 노출 vs 내부 동작 노출

공개 인터페이스 안에 있는 비공개 세부사항까지 테스트할 경우 그 자체로 구현 세부사항과 테스트가
결속하게 되므로, 세부사항이 변경될 때마다 테스트 코드는 깨진다.

내부의 세부사항을 테스트하는 것은 저품질로 이어질 수 있다. 코드의 작은 변화가 수많은 테스트를 깨면 (테스트 코드가 과도하게 내부적인 구현사항을 알고 있기 때문에) 프로그래머는 깨진 테스트를 고치면서 당황한다. 테스트가 더 많이 깨질수록 리팩토링이 꺼려진다. 리팩토링이 줄어들수록 코드 베이스는 빠르게 퇴화한다. 이러한 문제로 테스트 코드의 강한 결합성 때문에 단위 테스트에 시간을 투자하는 것을 거부하는 팀도 있다.

이외에도 테스트를 작성하기 위해 객체에 대해 과도하게 사적인 질문을 할 수 있다. 내부 필드에 대한 단언을 하려고 게터 메서드를 만들어야 할 수도 있다. 테스트를 위해 내부 데이터를 노출하는 것은 테스트와 프로덕션 코드 사이에 과도한 결합을 초래한다.

내부 동작을 테스트하려는 충동이 든다면, 설계에 문제가 있는 것이다.

## 테스트 메서드의 이름 짓기

테스트 메서드의 이름을 짓는 것은, 매우 중요하다. 프로덕션 코드를 이해하는데 도움을 주기 때문.

아래는 멋지고 설명적인 테스트 메서드 이름의 예시이다.
- doingSomeOperationGeneratesSomeResult
  - 어떤 동작을 하면, 어떤 결과가 나온다.
- someResultOccursUnderSomeCondition
  - 어떤 결과는 어떤 조건에서 발생한다.

혹은, BDD와 같은 given-when-then 형식을 사용하면 아래와 같다.
- givenSomeContextWhenDoingSomeBehaviorThenSomeResultOccurs
  - 주어진 조건에서 어떤 일을 하면 어떤 결과가 나온다.

보통은 BDD와 같이 지으면 읽기에 길고 복잡할 수 있다. 보통은, givenSomeContext 부분은 제거하여 테스트 독자가 너무 많은 일을 하지 않도록 해야 한다.
- whenDoingSomeBehaviorThenSomeResultOccurs
  - 어떤 일을 하면 어떤 결과가 나온다.

## 테스트를 의미있게 만들기

테스트가 어떤 일을 하는지 파악하기 어려워한다면, 주석을 다는 것 외에도 아래와 같은 사항을 고려하자.
- 지역 변수 이름 개선하기
- 의미 있는 상수 도입하기
- hamcrast matcher 사용하기
- 커다란 테스트를 작게 나누어 집중적인 테스트 만들기
- 테스트에 필요한 컨텍스트들을 미리 setup하기.