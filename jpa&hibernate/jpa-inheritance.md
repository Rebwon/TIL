## JPA Mapping

- 상속 관계 매핑
  - 상속 관계 매핑의 종류는 SINGLE_TABLE, JOINED, TABLE_PER_CLASS가 있다.
  - 상속 관계 매핑의 DEFAULT는 SINGLE_TABLE이다.
  - 상속 관계 매핑을 사용하려면 @Inheritance 애노테이션을 붙여줘야한다.
  
아래의 예제는 상속 관계 매핑을 통한 INSERT를 하는 경우이다.

```java
@Entity
@Table(name = "ITEM")
@Getter
@NoArgsConstructor
@Inheritance(strategy = SINGLE_TABLE)
@DiscriminatorColumn
public class Item extends BaseEntity {
    private String name;
    private int price;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }
}

@Entity
@Getter
@NoArgsConstructor
@DiscriminatorValue("BOOK")
public class Book extends Item {
    private int i18n;

    public Book(String name, int price, int i18n) {
        super(name, price);
        this.i18n = i18n;
    }
}

...

@Test
public void insert() {
    itemRepository.save(new Book("광개토대왕", 15000, 31872830));
}
```

데이터를 INSERT 해줄 때, 상속 받은 엔티티의 컬럼도 같이 넣어줘야 하므로,
위와 같이 작성해야 한다.