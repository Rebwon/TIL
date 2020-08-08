## Kotlin

좋은 책
- Kotlin in Action
- 코틀린 쿡 북

Kotlin에서 자료형은 무조건 자바에서의 Wrapper타입이다.
자료형이 Wrapper 타입이기 때문에 값, 참조를 둘다 비교가 가능하다.

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