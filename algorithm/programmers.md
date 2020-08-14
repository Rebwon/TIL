### 두 정수 사이의 합

문제 설명: 두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요.
      예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.
      
제한 조건:
- a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
- a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
- a와 b의 대소관계는 정해져있지 않습니다.

```java
// 처음 풀었던 방법
public long solution(int a, int b) {
        long answer = 0;

        if(a<b) {
            for(int i=a; i<=b; i++) {
                answer += i;
            }
        } else {
            for(int i=b; i<=a; i++) {
                answer += i;
            }
        }
        return answer;
    }
```

for를 사용하면 시간복잡도가 늘어나기 때문에 이후에 아래와 같은 방법으로 대체
```java
public long solution2(int a, int b) {
        return sumAtoB(Math.min(a, b), Math.max(b, a));
}

// 아래에 수식은 등차수열의 합 공식
private long sumAtoB(long a, long b) {
    return (b - a + 1) * (a + b) / 2;
}
```

코틀린 버전
```kotlin
fun solution(a: Int, b: Int) = sumAtoB(a.coerceAtMost(b).toLong(), a.coerceAtLeast(b).toLong())
fun sumAtoB(a: Long, b: Long) = (b - a + 1) * (a + b) / 2
```