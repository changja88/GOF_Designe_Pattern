반복자 (Iterator Pattern)
* 최종 정리
- 반복문에서 사용되는 i의 기능을 추상화 해서 일반화 한 패턴
- 무엇인가 많이 모여서 있는 것들을 순서대로지정하면서 젠체를 검색하는 처리를 싱행 하기 위한 패턴

```java
// 요소들이 나열되어 있는 '집합체'를 나타낸다
public interface Aggregate {
    // 이메소드는 집합체에 대응하는 Iterator를 1개 작성하기 위한 것이다
    public abstract Iterator iterator();
}

// 요소를 하나씩 나열하면서 루프 변수와 같은 역할을 수행한다
public interface Iterator {
    public abstract boolean hasNext();
    public abstract Object next();
}

public class Book {
    private String name;
    public Book(String name){
        this.name = name;
    }
    public String getName(){
        return name;
    }
}

public class BookShelf implements Aggregate {
    private Book[] books;
    private int last = 0;
    public BookShelf (int maxsize){
        this.books = new Books[maxsize];
    }
    public Book getBookAt(int index){
        return books[index];
    }
    public void appendBook(Book book){
        this.boos[last] = book;
        last++;
    }
    public int getLength(){
        return last;
    }
    public Iterator iterator(){
        return new BookShelfIterator(this);
    }
}

public class BookShelfIterator implements Iterator {
    private BookShelf bookShelf;
    private int index;
    public BOokShelfIterator(BookShelf bookShelf){
        this.bookShelf = bookShelf;
        this.index = 0;
    }
    public boolean hasNext(){
        if(index < bookShelf.getLength()){
            return true;
        }else {
            return false;
        }
    }
    public Object next(){
        book = bookShelf.getBootAt(index);
        index++;
        return book;
    }
}
public class Main {
    public static void main(String[] args){
        BookShlf bookShelf = new BookShelf(4);
        bookShelf.appendBook(new Book("Around the World in 80"));
        bookShelf.appendBook(new Book("Bible"));
        bookShelf.appendBook(new Book("Cinderella"));
        bookShelf.appendBook(new Book("Daddy-Long-Legs"));
        Iterator it = bookShelf.iterator();
        while(it.hasNext()){
            Book book = (Book)it.next();
            System.out.println(book.getName());
        }
    }
}
```

* GOF
1. 의도
- 내부 표현부를 노출하지 않고 어떤 집합 객체에 속한 원소들을 순차적으로 접근할 수 있는 ㅏㅂㅇ법을 제공한다

2. 다른이름
- 커서(Curso)

3. 동기
- 리스트등 집합 객체 들은 내부 표현 구조를 노출하지 않고도 자신의 원소를 접근할 수 있는 방법을 제공 하는게 좋다

4. 활용성
- 객체 내부 표현 방식을 모르고도 집합 객체의 각 원소들에 접근하고 싶을 때
- 집합 객체를 순회하는 다양한 방법을 지원하고 싶을 때
- 서로 다른 집합 객체 구조에 대해서도 동일한 방법으로 순회하고 싶을 때

5. 참여자
- Iterator
    - 원소를 접근하고 순회하는 데 필요한 인터페이스를 제공한다
- ConcreteIterator 
    - Iterator 에 정으된 인터페이슬르 구혀나는 클래스로, 순회 과정 중 집합 객체 내에서 현재 위치를 기억한다
- Aggregate
    - Iterator 객체를 생성하는 인터페이스를 정의 한다
- ConcreteAggregate
    - 해당하는 ConcreteIterator 의 인스턴스를 반환하는 Iterator 생성 인터페이스를 구현 한다

6. 결과
- 집합 객체의 단양한 순회 방법을 제공한다
    ->Iterator 는 순회 알고리즘을 바꿀 수 있다
- Iterator는 Aggregate 클래스의 인터페이스를 단순화한다
    ->Iterator의 순회 인터페이스는 Aggregate 클래스에 정의한 자신과 비슷한 인터페이스들을 없애서 Aggregate
      인터페이스를 단순화할 수 있다
- 집합 객체에 따라 하나 이상의 순회 방법이 제공될 수 있다
    -> 각 Iterator 마다 자신의 순회 상태가 있으므로 하나의 집합 객체를 한번에 여러 번 순회 시킬 수 있다