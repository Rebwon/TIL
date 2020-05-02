## Enumeration vs Iterator

출처 : whiteship 블로그

콜렉션 뷰에는 Enumeration과 Iterator 그리고 ListIterator가 있다.

Enumeration과 Iterator의 차이는 스냅샷 여부이다.

Enumeration은 콜렉션을 순회할 때 그 순간마다 새로 객체를 만드는 것 같다.
하지만 Iterator는 그렇지 않다.

> 콜렉션 뷰 객체인 Iterator와 ListIterator 객체가 fail-fast 방식인 이유는 스냅샷에 대한 보장을 포함하고있지 않기 때문이다. 즉, 콜렉션의 뷰인 Iterator 객체를 얻었을 때 그 순간의 상태를 따로 생성하지 않기 때문에,Iterator 객체에서 순차적으로 하부 콜렉션 객체를 접근할 때, 콜렉션 객체의 변경에 영향을 받게 된다.

그결과 스냅샷을 찍을 때 따로 객체를 생성하지 않고 원래 콜렉션 객체를 참조 하고 있는 Iterator를 사용하여 콜렉션의 요소들을 보고 있을 때 삭제 or 추가 가 발생할 때 데이타 무결성을 지키기 위해서 런타임 에러를 발생시킵니다. 이런것을 Fail-fast 방식이라고 합니다.

하지만 Enumeration은 그렇치 않겠죠. 그렇다면 'Iterator를 쓰면서 삭제 or 추가를 할 수는 없을까?' 라는 생각이 드는데요. 사실 제일 먼저 든 생각은 'List의 경우에 왜 Iterator를 사용하는 가?' 였습니다. List는 get(int) 메소드를 사용해서 요소들에 바로 접근을 할 수 있기 때문에 Iterator가 꼭 필요한가 라는 생각을 했었는데요. 
이건 List의 경우일 때이고 다른 Collection 인터페이스에는 get()이라는 메소드가 없었습니다. 그리고 iterator() 메소드는 있었지요. 그것을 보고 아.. 모든 콜렉션 들은 iterator() 만 알면 모든 요소들에 접근 할 수 있도록 하는 것인가.. 라는 자문자답을 해보게 되었습니다. 다시 원래 질문으로 돌아가서 'Iterator를 사용하면서는 삭제 or 추가를 할 수 없을까?' 라는 바보 같은 질문입니다. 
당연히 쓸수가 없지요. 런타임 에러가 발생한다고 했으니깐요. 하지만 스냅샷을 만들어 주면 어떨까요? 그런 방법이 아티클에서 소개되고 있었습니다.

```java
public Iterator snapshotIterator(Collection collection) {
    return new ArrayList(collection).iterator();
}
```

그리고 동기화에 대한 이야기 부분에서 '콜렉션 프레임워크는 동기화를 보장하고 있지 않다.' 라고 합니다. 하지만 Vector의 경우에는 예외라고 생각이 되네요. Vector 클래스에 보면 synchronized 키워드가 많이 있었습니다. 하지만 대부분의 콜렉션의 synchronized 키워드를 찾아 볼 수 없었습니다. 이 것은 동기화를 보장하고 있지 않다는 듯입니다. 
즉 동시에 쓰기와 읽기가 가능해 진다는 것인데 이럴 때 발생할 수 있는 문제를 해결하려면 역시 동기화 시키는 방법이 제일 좋겠지요. 그러한 해결책도 역시 아티클에서 소개 되고 있습니다.

```java
Collection c = Collections.synchronizedCollection(myCollection);
```
