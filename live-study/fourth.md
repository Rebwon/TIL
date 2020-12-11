## 4주차 

- 선택문
  - if, else, switch
- 반복문
  - while
  - do while
  - for

### 선택문

선택문은 if, else, switch가 있다.

아래는 가장 기본적인 if문이다.
```java
if(something) {
    // code
}
```

if - else 구조이다.
```java
if(something) {
    // code
} else {
    // code
}
```

if - else if - else 구조이다.
```java
if(something) {
    // code
} else if (something) {
    // code
} else {

}
```

실행 블록 {} 은 생략이 가능하다. 개인적인 코드 스타일은 if문을 사용하는 것을 최대한 피하고 되도록이면 1개만 사용하기 때문에
아래와 같이 실행 블록은 생략하는편이다.
```java
if(something)
    throw new IllegalArgumentException();
```

switch문은 기본형 타입으로 동작하며, Enum도 사용이 가능하며 Wrapper 클래스도 사용 가능하다.
```java
switch(something) {
    // case에 특정한 값을 확인한다.
    case: 
        // 이곳에서 특정한 조건을 실행
        break;
    default:
    // 모든 케이스에 맞지 않을 경우 기본 조건 실행
}
```

### 반복문

기본적인 while문이다. 

```java
while(true) {
    // code
}
```

먼저 어떠한 코드를 실행하고 그 후에 반복을 결정하고 싶을 때, do while을 사용한다.

```java
do {
    // code
} while(true);
```

기본적인 for문이다.
```java
// 초기화식; 조건식; 증감식으로 구성된다.
for(int i=0; i<5; i++) {
    // code
}
```

for문을 사용할 때 유의점.
- 초기화식, 조건식, 증감식이 모두 필요하진 않을 경우 안써도 된다.
- 증감식에 증감 연산자를 사용할 필요가 없다면 안써도 된다.

향상된 for문 : 어떠한 콜렉션 혹은 배열을 차례대로 반복할 경우 아래와 같이 사용하는 것이 가독성이 좋다.
```java
for(String s : strings) {

}
```