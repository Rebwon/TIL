## Feature Envy 

Feature Envy는 한국어로 기능에 대한 욕심이라는 뜻이다.

객체의 가장 중요한 요점은 데이터와 데이터를 사용하는 프로세스를 하나로 묶는 기술이다. 

한가지 고전적인 냄새가 있는데 그것은 메소드가 자신이 속한 클래스보다 다른 클래스에 관심을 가지고 있는 경우이다.

- 가장 흔한 욕심이 데이터에 대한 욕심이다. Move Method를 사용한다.
- 메소드의 특정 부분만 이런 욕심으로 고통 받는데 이럴 때는 욕심이 많은 부분에 대해서 Extract Method 사용한 다음 Move Method를 사용한다.
- 물론 이런 규칙이 깨지는 몇몇 복잡한 패턴도 있다. 디자인 패턴에서 Strategy와 Visitor 가 당장 떠오른다. Kent Beck의 Self Delegation도 그 중 하나다.
- 확산적 변경과 싸우기 위해 이런 것들을 이용해야 한다. 가장 기본적인 규칙은 같이 변하는 것을 모으는 것이다.
- 데이터와 그 데이터를 이용하는 동작은 보통 같이 변하지만 예외도 있다. 이런 예외의 경우는 동작을 옮겨서 한 곳에서만 변하도록 한다. Strategy와 Visitor는 인디렉션을 사용하여 오버라이드 되어야 하는 약간의 기능을 독립시킴으로써 동작을 쉽게 변경할 수 있다.

출처 : https://opennote46.tistory.com/150