## 2주차 과제: 자바 데이터 타입, 변수 그리고 배열

## Table of contents

- 프리미티브 타입과 레퍼런스 타입
- 프리미티브 타입 종류와 값의 범위 그리고 기본 값
- 리터럴
- 변수 선언 및 초기화하는 방법
- 변수의 스코프와 라이프타임
- 타입 변환, 캐스팅 그리고 타입 프로모션
- 1차 및 2차 배열 선언하기
- 타입 추론, var

### 프리미티브 타입과 레퍼런스 타입

- Primitive 타입은 boolean 타입과 numeric 타입으로 이루어져있다. numeric 타입은 byte, short, int, long, char, float, double이 있다.
- Reference 타입은 class 타입, interface 타입, array 타입이 있다. null 타입도 존재한다. Reference 타입은 동적으로 생성된 클래스 유형 또는 동적으로 생성된 배열 인스턴스입니다. Reference 타입의 값은 개체에 대한 참조이다. array를 포함한 모든 개체는 클래스 개체 방법을 지원한다. 문자열 리터럴은 문자열 개체로 표시된다. Reference Type의 기본 값은 null.

#### Variables of Primitive Type
Primitive 유형 변수는 해당 Primitive 유형의 값을 유지합니다.

#### Variables of Reference Type

클래스 유형 T의 변수는 클래스 T 인스턴스 또는 클래스 T의 하위 클래스인 모든 클래스에 대한 null 참조 또는 참조를 포함할 수 있습니다.

인터페이스 유형의 변수는 인터페이스를 구현하는 클래스의 모든 인스턴스에 대한 null 참조 또는 참조를 포함할 수 있습니다.

변수는 항상 선언된 유형의 하위 유형을 참조할 수 있는 것은 아니며 선언된 유형의 하위 클래스 또는 하위 인터페이스만 참조할 수 있습니다. 이는 아래에서 논의되는 힙 오염 가능성 때문입니다.

T가 원시 유형인 경우, "T 배열" 유형의 변수는 "T 배열" 유형의 배열에 대한 null 참조 또는 참조를 포함할 수 있습니다.

T가 참조 유형인 경우, "T array" 유형의 변수는 S 유형이 T 유형의 하위 클래스 또는 하위 인터페이스인 경우, " array of S" 유형의 array에 대한 참조 또는 null 참조를 포함할 수 있습니다.

개체[] 유형의 변수는 모든 참조 유형의 배열에 대한 참조를 포함할 수 있습니다.

개체 유형의 변수는 클래스의 인스턴스(instance)인지 Array(instance)인지 여부에 관계없이 모든 개체에 대한 null 참조 또는 참조를 포함할 수 있습니다.

매개 변수 형식의 변수는 해당 매개 변수 유형이 아닌 개체를 나타낼 수 있습니다. 이 상황은 힙 오염이라고 알려져 있습니다.

힙 오염은 프로그램이 컴파일 타임 체크되지 않은 경고를 발생시키는 원시 유형과 관련된 작업을 수행한 경우에만 발생할 수 있습니다.

### 프리미티브 타입 종류와 값의 범위 그리고 기본 값

Primitive 타입은 다른 Primitive 값과 상태를 공유하지 않는다. numeric 유형은 정수 유형과 부동 소수점 유형으로 이루어져 있다.

정수 유형은 byte, short, int, long이며 값은 각각 8비트, 16비트, 32비트, 64비트 두개의 완성형 정수로 서명된다. 값은 UTF-16 코드로 비서명된 16비트 정수이다.

부동 소수점 유형은 32비트 IEEE 754 부동 소수점 숫자를 포함하는 float type과 64비트 IEEE 754 부동 소수점 숫자를 포함하는 double type입니다.

부울 유형에는 true와 false의 두 가지 값이 있습니다.

Primitive 종류 및 값의 범위
- byte -128~127
- short -32768~32767
- int -2147483648~2147483647
- long -9223372036854775808~9223372036854775807
- char 0~65535

Primitive 타입 기본 값
- byte : 0
- short : 0
- int : 0
- long : 0l
- float: 0.0f
- double : 0.0
- boolean : '\u0000'
- char : false


### 리터럴

리터럴 기본 유형
- IntegerLiteral
- FloatingPointLiteral
- BooleanLiteral
- CharacterLiteral
- StringLiteral
- NullLiteral

정수 리터럴
- 정수 리터럴은 십진수, 16진수, 8진수, 2진수로 표현할 수 있습니다.
- 정수 리터럴은 ASCII 문자 L 또는 l 접미사가 붙은 Long형과 그렇지 않은 경우 int형입니다.
- 문자 l은 숫자 1과 구분하기 어려운 경우가 많기 때문에 L이 선호됩니다.
- 16진수 리터럴(0x) 2진수 리터럴(0b) 십진수, 8진수 리터럴(0) 

```
Example int literals
0    2    0372    0xDada_Cafe    1996    0x00_FF__00_FF
Example long literals
0l    0777L    0x100000000L    2_147_483_648L    0xC0B0L
```

부동 소수점 리터럴
- ASCII 문자 F또는 f 접미사가 붙은 경우 float 유형입니다. 그렇지 않은 경우 double형입니다. double형은 D 또는 d 접미사가 붙습니다.

```
Example float literals
1e1f    2.f    .3f    0f    3.14f    6.022137e+23f
Example double literals
1e1    2.    .3    0.0    3.14    1e-9d    1e137
```

Boolean 리터럴
- ASCII 문자로 true, false 두 유형의 리터럴이 있습니다.

Char 리터럴
- Char리터럴은 문자 또는 EscapeSequence로 표현되며, ASCII 작은 따옴표('')로 묶입니다.
- Char리터럴은 UTF-16 코드 단위만 나타낼 수 있습니다. 
- Char리터럴은 \u0000 ~ \uffff의 값으로 제한됩니다.
- '' 작은 따옴표로 문자 시작과 끝을 감싸주지 않으면 컴파일 오류가 발생합니다.
- CR과 LF 문자는 입력 문자가 아닙니다. 각 문자는 LineTerminator를 구성하는 것으로 인식됩니다. 따라서 '\u000a'를 쓰는게 아닌, '\n'을 사용해야 합니다. '\u000d' 이것도 마찬가지로 '\r'을 사용해야 합니다.

```
Example char literals
'a'

'%'

'\t'

'\\'

'\''

'\u03a9'

'\uFFFF'

'\177'

'™'
```

String 리터럴
- String 리터럴은 "" 큰따옴표로 묶인 0개 이상의 문자로 구성됩니다.
- ""로 문자의 시작과 끝을 감싸주지 않을 경우 컴파일 오류가 발생합니다.
- Char 리터럴과 마찬가지로 CR, LF 문자는 입력문자가 아닙니다.
- 긴 문자열 리터럴은 항상 더 짧게 분할할 수 있으며, 문자열 연결 연산자 + 를 사용하여 작성할 수 있습니다.
- String 리터럴은 Class String 인스턴스에 대한 참조입니다.
- String 리터럴은 항상 동일한 String 인스턴스를 나타냅니다. 

```
Example string literals

""                    // the empty string
"\""                  // a string containing " alone
"This is a string"    // a string containing 16 characters
"This is a " +        // actually a string-valued constant expression,
    "two-line string"    // formed from two string literals
```

```java
// Example String Class Instance Same
package testPackage;
class Test {
    public static void main(String[] args) {
        String hello = "Hello", lo = "lo";
        System.out.println(hello == "Hello");
        System.out.println(Other.hello == hello);
        System.out.println(other.Other.hello == hello);
        System.out.println(hello == ("Hel"+"lo"));
        System.out.println(hello == ("Hel"+lo));
        System.out.println(hello == ("Hel"+lo).intern());
    }
}
class Other { static String hello = "Hello"; }
```

```java
package other;
public class Other { public static String hello = "Hello"; }
```

위 코드에 대한 결과

```
true
true
true
true
false
true
```

위 예제가 시사하는 바는 다음과 같습니다.
- 동일한 클래스 및 패키지의 문자열 리터럴은 동일한 문자열 개체(제4.3.1조)에 대한 참조를 나타냅니다.
- 동일한 패키지의 서로 다른 클래스에 있는 문자열 리터럴은 동일한 문자열 개체에 대한 참조를 나타냅니다.
- 서로 다른 패키지의 서로 다른 클래스에 있는 문자열 리터럴도 동일한 문자열 개체에 대한 참조를 나타냅니다.
- 상수 식(제15.28)으로 연결된 문자열은 컴파일할 때 계산된 다음 리터럴인 것처럼 처리됩니다.
- 런타임에 연결하여 계산된 문자열이 새로 생성되므로 구별됩니다.
- 계산된 문자열을 명시적으로 상호 연결한 결과는 동일한 내용을 가진 기존의 문자열 리터럴과 동일한 문자열 개체입니다.

Null 리터럴
- Null 리터럴은 하나의 값, 즉 null 참조를 가지며, ASCII 문자 null로 표현됩니다.

### 변수 선언 및 초기화하는 방법

변수를 선언하는 것은, Stack 영역에 메모리를 할당하는 것입니다.

```java
class Point {
    int x, y; // 변수 선언
    Point() { System.out.println("default"); }

    // 선언된 변수를 생성자를 통해 초기화
    Point(int x, int y) { this.x = x; this.y = y; }

    /* Point 인스턴스는 클래스 초기화 시간에 명시적으로 생성됨 */
    static Point origin = new Point(0,0);

    /* 문자열은 + 연산자에 의해 암시적으로 생성됨 */
    public String toString() { return "(" + x + "," + y + ")"; }
}

class Test {
    public static void main(String[] args) {
        /* Point는 newInstance를 사용하여 명시적으로 생성됨 */
        Point p = null;
        try {
            p = (Point)Class.forName("Point").newInstance();
        } catch (Exception e) {
            System.out.println(e);
        }

        /* 배열은 배열 생성자에 의해 암시적으로 생성됨 */
        Point a[] = { new Point(0,0), new Point(1,1) };

        /* Strings are implicitly created 
           by + operators: */
        System.out.println("p: " + p);
        System.out.println("a: { " + a[0] + ", " + a[1] + " }");
    
        /* 배열은 배열 생성 표현식에 의해 명시적으로 생성됨 */
        String sa[] = new String[2];
        sa[0] = "he"; sa[1] = "llo";
        System.out.println(sa[0] + sa[1]);
    }
}
```

```java
// 객체 참조 상태 확인
class Value { int val; }

class Test {
    public static void main(String[] args) {
        int i1 = 3;
        int i2 = i1;
        i2 = 4;
        System.out.print("i1==" + i1);
        System.out.println(" but i2==" + i2);
        Value v1 = new Value();
        v1.val = 5;
        Value v2 = v1;
        v2.val = 6;
        System.out.print("v1.val==" + v1.val);
        System.out.println(" and v2.val==" + v2.val);
    }
}
```

아래는 실행 결과
```
i1==3 but i2==4
v1.val==6 and v2.val==6
```

v1,v2는 new를 통해 생성된 하나의 값 개체로써 동일한 인스턴스 변수를 참조하기 때문에 6이라는 값이 나왔지만, i1,i2는 서로 다른 변수이기 때문에 다른 값을 나타낸다.

### 변수의 스코프와 라이프타임

변수에는 다음과 같은 8가지 종류가 있습니다.

- 클래스 변수는 클래스 선언 내에서 또는 인터페이스 선언 내에서 static 키워드를 사용하거나 사용하지 않고 선언된 필드입니다. 클래스 변수는 클래스 또는 인터페이스가 준비되고 기본값으로 초기화 되면 생성됩니다. 클래스 또는 인터페이스가 언로드될 때 클래스 변수는 사실상 더이상 존재하지 않습니다.
- 인스턴스 변수는 static 키워드를 사용하지 않고 클래스 선언 내에서 선언된 필드입니다. 클래스 T에 인스턴스 변수인 필드 a가 있는 경우, 클래스 T의 각 새로 생성된 개체 또는 T의 하위 클래스인 클래스의 일부로 새 인스턴스 변수가 생성되고 기본값으로 초기화됩니다. 인스턴스(instance) 변수는 객체의 필요한 최종화가 완료된 후 해당 객체가 필드인 객체를 더 이상 참조하지 않을 때 사실상 존재 중지됩니다.
- 배열 구성 요소는 배열인 새 객체가 생성될 때마다 기본값으로 생성 및 초기화되는 이름 없는 변수입니다. 배열이 더 이상 참조되지 않으면 배열 구성 요소가 실제로 중지됩니다.
- 메서드 선언에 선언된 모든 매개 변수에 대해 메서드가 호출될 때마다 새 매개변수가 생성됩니다. 메서드 호출의 해당 인수 값으로 새 변수가 초기화됩니다. 메서드의 본문 실행이 완료되면 메서드 매개변수가 실제로 더 이상 존재하지 않습니다.
- 생성자 선언에 선언된 모든 매개 변수에 대해 클래스 인스턴스 생성식 또는 명시적 생성자 호출이 해당 생성자를 호출할 때마다 새 매개 변수 변수가 생성됩니다. 새 변수는 생성 식 또는 생성자 호출의 해당 인수 값으로 초기화됩니다. 생성자 본문의 실행이 완료되면 생성자 매개 변수가 실제로 더 이상 존재하지 않습니다.
- 람다 식으로 선언된 모든 파라미터에 대해 람다 본문에 의해 구현된 메서드가 호출될 때마다 새 파라미터 변수가 생성됩니다. 메서드 호출의 해당 인수 값으로 새 변수가 초기화됩니다. 람다 표현식 본문의 실행이 완료되면 람다 매개변수가 실제로 더 이상 존재하지 않습니다.
- 예외 파라미터는 try의 catch 절에 예외가 포착될 때마다 생성됩니다. 새 변수는 예외와 관련된 실제 개체로 초기화됩니다. catch 절과 연결된 블록의 실행이 완료되면 예외 파라미터가 실제로 더 이상 존재하지 않습니다.
- 로컬 변수는 로컬 변수 선언문에 의해 선언됩니다. 제어 흐름이 블록 또는 문에 들어갈 때마다 해당 블록에 즉시 포함된 로컬 변수 선언문에 선언된 각 로컬 변수에 대해 새로운 변수가 생성됩니다.

로컬 변수 선언문에는 변수를 초기화하는 식이 포함될 수 있습니다. 그러나 초기화 식이 있는 로컬 변수는 실행을 선언하는 로컬 변수 선언문이 나올 때까지 초기화되지 않습니다. (확정 할당 규칙은 로컬 변수가 초기화되거나 값이 할당되기 전에 값을 사용하지 못하도록 합니다.) 블록 실행 또는 명령문 실행이 완료되면 로컬 변수는 실제로 더 이상 존재하지 않습니다.

예외적인 한 가지 상황이 아니라면 로컬 변수 선언문을 실행할 때 항상 로컬 변수가 생성되는 것으로 간주할 수 있습니다. 예외적인 상황은 스위치 문과 관련됩니다. 여기서 제어는 블록에 들어갈 수 있지만 로컬 변수 선언문의 실행을 우회할 수 있습니다. 그러나 확정 할당 규칙에 의해 부과되는 제한 때문에, 이러한 우회된 로컬 변수 선언문에 의해 선언된 로컬 변수는 할당 식에 의해 확실히 값을 할당받기 전에는 사용할 수 없습니다.

```java
//Example Different Kinds of Variables
class Point {
    static int numPoints;   // numPoints is a class variable
    int x, y;               // x and y are instance variables
    int[] w = new int[10];  // w[0] is an array component
    int setX(int x) {       // x is a method parameter
        int oldx = this.x;  // oldx is a local variable
        this.x = x;
        return oldx;
    }
}
```

### 타입 변환, 캐스팅 그리고 타입 프로모션

타입 변환은 어떤 값이나 변수의 타입을 다른 타입으로 변경하는 것입니다.

타입은 두 가지 방향으로 변환됩니다. 명시적 형변환, 묵시적 형변환.

묵시적 형변환
- 두 데이터 타입은 호환 되어야 합니다.
- 더 작은 데이터 타입의 값을 더 큰 범위의 타입에 할당할 때만 동작합니다.

명시적 형변환
- 더 작은 범위의 타입에 더 큰 범위의 타입 값을 할당하기 위해선 형변환을 명시해줘야 합니다.
- 호환되지 않는 데이터 타입에도 사용 가능합니다.

타입 프로모션
- 표현식을 평가할 때, 중간에 피연산자 범위를 초과할 수 있기 때문에 자동으로 값이 승격되는데 이를 타입 프로모션이라 합니다.
- byte, short, char를 식 평가시 자동으로 int로 프로모션됩니다.
- long, float, double인 경우 전체 표현식이 선언된 타입으로 프로모션 됩니다.

Primitive 타입 값을 Reference 타입 값으로 자동 변환해주는 것을 오토 박싱, 그 반대를 언박싱이라고 합니다.

오토 박싱과 언박싱은 성능에 영향을 주기 때문에 성능이 중요한 애플리케이션에서는 신경써서 작성해야 합니다.


### 1차 및 2차 배열 선언하기

1차 배열 선언
```
T name[];
T[] name;
```

2차 배열 선언
```
T name[][];
T[][] name;
```

### 타입 추론, var

Java 10부터 지원하는 기능입니다. 컴파일러가 자료형을 추론합니다.

```java
var a = 1;            // Legal Case
var b = 2, c = 3.0;   // Illegal Case : 여러개의 변수를 선언하고 초기화 했습니다.
var d[] = new int[4]; // Illegal Case : 배열 선언이 불가합니다.
var e;                // Illegal Case : 잘못된 초기화
var f = { 6 };        // Illegal Case : {}로 감쌀수 없습니다.
var g = (g = 7);      // Illegal Case : 초기화의 자체 참조가 잘못되었습니다.

```