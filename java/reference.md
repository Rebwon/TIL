## Reference Object 활용

Rechability(접근성)이라는 단어를 처음 보게 됐습니다. 어떤 객체에 접근할 수 있는 reference의 강도를 네 단계로 구분 해 두었습니다.

Strong reference > Soft reference > Weak reference > Phantom reference

Strong reference는 new를 사용하여 객체를 생성할 때 생기는 reference를 말하며 어떤 객체를 참조 하고 있는 Strong reference가 존재하는 이상 그 객체는 GC의 수거 대상이 되지 않습니다.

Soft reference는 GC의 대상이 될 수도 있고 안 될 수도 있는데 Out of memory가 발생할 상황이라면 무조건 수거가 되고 그렇치 않다면 살아 남는 레퍼런스 입니다.

Weak reference는 GC가 발동하기 전까지는 살아 있지만 GC가 일단 발동하는 순간 제거가 되버리는 레퍼런스 입니다.(맨 끝에 사용되는 예제가 나옵니다.)

Phantom reference는 이 레퍼런스를 통해 객체에 접근하려고 하면 null 값만 리턴합니다. 즉 접근할 수가 없습니다. 그렇다고 객체가 없어진 것은 아니고 다만 객체가 수거 될 때 사후 처리를 위해 사용한다고 합니다.

Strong reference를 생성하는 방법은 일반적인 객체의 레퍼런스를 생성하는 방식과 같으며
Strong reference를 제외한 다른 reference들은 모두 Soft reference를 생성하는 방식으로 만들어집니다.

Date endDate = new Date();
//Date 객체의 접근성은 Strongly Rechable 하다고 할 수 있겠습니다.

SoftReference sr = new SoftReference(endDate);
//Date 객체의 soft reference는 위의 방식 처럼 생성합니다. 하지만 여전히 Date 객체의 Strong reference(endDate)가 존재하기 때문에 이 객체의 접근성은 Strongly Rechable 합니다.

endDate = null;
//이 순간 Strong reference는 끊기게 되며 이제 이 객체의 접근성은 Softly rechable 하다고 할 수 있겠습니다.

아래는 Weak Reference를 활용하는 예제.

```java
public class SimpleCache {
   private Map map = new HashMap();

   public void put(String key, Object value) {
        // new WeakReference()를 통해 객체 참조를 
        // Weak reachable 상태로 만든다.
        // 이때 WeakReference 객체 자체는
        // Strong reachable 상태이다.
        // 따라서 GC의 대상이 아니다.
       map.put(key, new WeakReference(value));
   }

   public Object get(String key) {
       Object obj = map.get(key);
       return ((WeakReference) obj).get();
   }
}
```

```java
public class SimpleCacheTest {
   public void testCache() {
       String key = "Key";
       String value = "" + Math.random();

       SimpleCache cache = new SimpleCache();
       cache.put(key, value);

       value = null;

       assertNotNull(cache.get(key));   // Garbage collection 이전에는 value가 존재함.
       System.gc();     // 여기서 GC를 호출함으로써 기존에 Weak reachable 상태인 cache.get(key)가 사라지게 된다.
       assertNull(cache.get(key));        // value가 사라짐.
   }
}
```

위와 같은 동작을 통해 LRU 캐시와 같은 임시 객체 보관 구조를 쉽게 만들 수 있다.

추가로 Java GC는 root set으로부터 시작해서 객체에 대한 모든 경로를 탐색하고, 그 경로에 있는 reference object들을 조사하여 그 객체에 대한 reachability를 결정한다. 다양한 참조 관계의 결과, 하나의 객체는 다음 5가지 reachability 중 하나가 될 수 있다.

- strongly reachable: root set으로부터 시작해서 어떤 reference object도 중간에 끼지 않은 상태로 참조 가능한 객체, 다시 말해, 객체까지 도달하는 여러 참조 사슬 중 reference object가 없는 사슬이 하나라도 있는 객체

- softly reachable: strongly reachable 객체가 아닌 객체 중에서 weak reference, phantom reference 없이 soft reference만 통과하는 참조 사슬이 하나라도 있는 객체

- weakly reachable: strongly reachable 객체도 softly reachable 객체도 아닌 객체 중에서, phantom reference 없이 weak reference만 통과하는 참조 사슬이 하나라도 있는 객체

- phantomly reachable: strongly reachable 객체, softly reachable 객체, weakly reachable 객체 모두 해당되지 않는 객체. 이 객체는 파이널라이즈(finalize)되었지만 아직 메모리가 회수되지 않은 상태이다.

- unreachable: root set으로부터 시작되는 참조 사슬로 참조되지 않는 객체

GC가 객체를 처리하는 순서는 항상 아래와 같다.

1. soft references
2. weak references
3. 파이널라이즈
4. phantom references
5. 메모리 회수

즉, 어떤 객체에 대해 GC 여부를 판별하는 작업은 이 객체의 reachability를 strongly, softly, weakly 순서로 먼저 판별하고, 모두 아니면 phantomly reachable 여부를 판별하기 전에 파이널라이즈를 진행한다. 그리고 대상 객체를 참조하는 PhantomReference가 있다면 phantomly reachable로 간주하여 PhantomReference를 ReferenceQueue에 넣고 파이널라이즈 이후 작업을 애플리케이션이 수행하게 하고 메모리 회수는 지연시킨다.

추가로 읽을 거리  
https://d2.naver.com/helloworld/329631