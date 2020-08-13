## Kotlin

좋은 책
- Kotlin in Action
- 코틀린 쿡 북

코틀린 자료형
- Kotlin에서 자료형은 무조건 자바에서의 Wrapper타입이다.
- 자료형이 Wrapper 타입이기 때문에 값, 참조를 둘다 비교가 가능하다.

값 비교는 == 참조 비교는 ===이다.

모던한 언어 답게, ${}가 지원된다(자바스크립트와 동일)

변수 선언 방법
```kotlin
// 묵시적이면서 가변적인 변수 선언
var email = "msolo021015@gmail.com"

// 명시적이면서 불변적인 변수 선언
val email2: String = "msolo021015@gmail.com"

println("참조 비교: ${email === email2}")
println("값 비교: ${email == email2}")

//값이 하나일 땐 $변수명 으로 사용가능
println("email : $email")

// 자바에서 mockMvc로 HTTP Method 안에 url을 매핑할때 불편한 점이 코틀린에서는 간결하게 작성된다.
val url = "/api/posts"
val id = 1L
println("url: ${url + id}")
// url: /api/posts/1
```

null을 허용한 변수 선언
```kotlin
// 기본 선언은 null이 허용되지 않는다.
// null이 가능한 형식은 데이터형?와 같이 선언해야한다.
var str1: String
str1 = null

val a: Int? = null
var b: String? = null

var str2: String?
str2 = null
println("str2: $str2, length: ${str2?.length}")
```

?.은 세이프콜이다. str2?.length라는 의미는 str2가 만약 null이면 뒤의 문장을 실행하지 않는다는 것이다.
!!.은 NotNull 단정 기호이다. 널이 아닐것이라고 단정해서 컴파일러가 오류를 무시한다.

```kotlin
fun main() {
    var str1: String?
    str1 = null
    val len = if(str1 != null) str1.length else -1
    println("str1: $str1, length: $len")
}
```

코틀린은 val에 조건문을 넣어서 사용할 수 있다.

위와 같은 방식은 조금 더 자바스러운 방식이고 코틀린에서는 elvis 표현식으로 간결하게 사용가능하다.
```kotlin
fun main() {
    var str1: String?
    str1 = null
    val len = str1?.length ?: -1
    println("str1: $str1, length: $len")
}
```

?:은 elvis 표현식이다. 앞에 조건이 null이면 뒤에 코드가 수행된다. 삼항 연산자와 유사한 문법인거 같다.

코틀린에서는 기본적으로 NotNull이며 Nullable일 경우 ?가 사용된다.

```kotlin
// a는 NotNull b는 Nullable
fun set(a: String, b: String?) {
    // Do Something
}
```

- 코틀린의 자료형은 모두 참조형으로 선언된다. 하지만 컴파일을 거쳐서 최적화될 때는, 참조형 자료형에서 기본형 자료형으로 변환된다.
- 코틀린에서는 자료형이 다른 변수에 재할당하면 자동 형 변환이 되지 않고 자료형 불일치 오류(type mismatch)가 발생한다.
```kotlin
val a: Int = 1 // 정수형 변수 a 를 선언하고 1을 할당
val b: Double = a // 자료형 불일치 오류 발생
val c: Int = 1.1 // 자료형 불일치 오류 발생 
```

만약 자료형을 변환해 할당하고 싶다면, 자료형 변환 메서드를 사용해야 한다.
- toByte: Byte
- toLong: Long
- toShort: Short 
- toFloat: Float
- toInt: Int
- toDouble: Double
- toChar: Char

스마트 캐스팅
- 값이 정수 혹은 실수일 경우, Number형을 사용하면 자료형이 값에 따라 자동적으로 변환된다.

묵시적 변환
- Any형은 자료형이 정해지지 않은 경우 사용한다. 코틀린의 Any형은 모든 클래스의 뿌리이다. Int나 String 그리고 사용자가 직접 만든 클래스까지 모두 Any형의 자식 클래스이다. 
즉,코틀린의 모든 클래스는 바로 이 Any 형이라는 슈퍼 클래스(Superclass)를 가진다. 따라서 Any를 사용해 변수를 선언하면 해당 변수는 어떤 형으로도 변환할 수 있게 되는 것이다.
- 자바의 Object가 최상위 클래스이듯 코틀린에서는 Any라는 슈퍼 클래스를 사용하는 것 같다.(뇌피셜)

%(모듈러) 연산으로 짝수와 홀수를 구분하는 것 외에 랜덤 난수 생성 시 범위 지정에 사용이 가능하다.

코틀린 함수 정의
```
fun 함수 이름([변수 이름: 자료형, 변수 이름: 자료형..]  ): [반환값의 자료형] { 
    표현식...
    [return 반환값] 
}
```

```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```
자바에서는 반환할 것이 없을 때, Void를 사용한다. 코틀린에서는 이와 유사한 Unit이라는 것을 제공하는데 반환을 아예 안하는 것이 아닌 Unit을 반환하는 것이다.
```kotlin
fun nothing(a: Int, b: Int): Unit {
    //return a + b <- 컴파일 에러
}
```

코틀린에서 가변인자를 받을 때는 아래와 같다.
```kotlin
fun sum(vararg nums: Int): Int {
    var result = 0
    for(num in nums) {
        result += num
    }
    return result
}
```

매개변수의 기본 값
```kotlin
fun add(name: String, email: String = "msolo021015@gmail.com") {

}

add("rebwon")   // email 인자를 생략하고 호출
```

매개변수 이름과 함께 함수 호출하기
```kotlin
fun namedParam(x: Int = 100, y: Int = 200, z:Int) {
    println(x + y + z)
}

namedParam(x = 200, z = 100)    // x,z의 이름과 함께 함수 호출(y는 기본값)
```

가변인자를 가진 함수
```kotlin
fun main(args: Array<String>) {
    normalVarargs(1, 2, 3, 4) // 4개의 인자 구성
    normalVarargs(4, 5, 6)    // 3개의 인자 구성
}

fun normalVarargs(vararg counts: Int) {
    for (num in counts) {
        println("$num")
    }
    print("\n")
}
```

순수 함수(Side-Effect가 없는 함수)
- 동일한 입력 인자에 대해서는 항상 같은 결과를 출력 혹은 반환
- 값이 예측이 가능해 결정적이다.

```kotlin
// 순수 함수의 예
fun sum(a: Int, b: Int) {
    return a + b    // 동일한 인자인 a,b를 받아 항상 a+b를 출력
}
```
순수 함수의 조건
- 같은 인자에 대하여 항상 같은 값을 반환
- 함수 외부의 어떤 상태도 바꾸지 않는다 ==> 외부의 값이나 외부의 상태에 영향을 받지 않고 주지 않는다.

순수 함수가 아닌 경우
```kotlin
fun checkEmailToken() {
    val token = User.getToken() // 외부의 User 객체 접근
    val dbToken = DB.getToken() // 외부의 DB 접근
    if(token.equals(dbToken)) process(something) // 외부에 접근한 변수의 값에 따라 달라짐
}
```

순수 함수 사용 이유
- 입력과 내용을 분리하고 모듈화 하므로 재사용성이 높아진다.
  - 여러 함수들과 조합해도 부작용이 없다.
- 특정 상태에 영향을 주지 않으므로 병행 작업 시 안전하다.
- 함수의 값을 추적하고 예측할 수 있기 때문에 테스트, 디버깅이 유리하다

함수형 프로그래밍에 적용 시
- 함수 자체를 매개변수, 인자에 혹은 반환 값에 적용한다(고차함수)
- 함수를 변수나 데이터 구조에 저장한다

람다식의 이용
- 람다식은 고차 함수에서 인자로 넘기거나 결과값으로 반환 등을 할 수 있다.

일급 객체
- 일급 객체는 함수의 인자로 전달 가능하다
- 일급 객체는 함수의 반환값에 사용 가능하다
- 일급 객체는 변수에 담을 수 있다
- 코틀린에서 함수는 1급 객체로 다룬다 1급 함수라고도 한다.

고차 함수
```kotlin
fun main() {
    println(highFun({x, y -> x + y}, 10, 20)) // 람다식 함수를 인자로 넘김
}

fun highFun(sum: (Int, Int) -> Int, a: Int, b:Int): Int = sum(a,b)  // sum 매개변수는 함수
```

고차 함수를 사용해서 람다식을 표현할 때
```kotlin
fun highFun(a: Int, b: Int, sum: (Int, Int) -> Int): Int {
    return sum(a, b)
}

fun main() {
    val result = highFun(10, 20) {x, y
        -> x + y
    }
    println(result)
}
```
위와 같이 매개 변수 함수를 뒤로 옮기면 함수를 선언할 때 {}로 따로 빼낼 수 있고, 람다식이 복잡해질 경우
더 가독성있게 코드 작성이 가능하다.