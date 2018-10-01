팩토리 메소드(Factory Method)
*최종정리
-> Concret Creator 종류가 다양하게 있는데 그중에 하나가 creator가 되어서 porduct를 생산한다
    Creator Group                      
      ACreator                          |----> Product A
      BCreator     ----->  Creator  ----|----> Product B          
      CCreator                          |----> Product C
                          전달받은 Creator 를 이용 하여 그 creatoer 가 만들수 있는 product 하나를 만든다

* Inflearn
- 팩토리 메소드 패턴에서 '템플릿 메소드 패턴이 사용됨'을 안다
- 팩토리 메소드 패턴에서의 '구조와 구현의 분리'를 이해하고 구조와 구현의 분리의 장점을 안다
  (사용될 기능과 그 기능을 만드는 구현의 분리 -> 모든 패턴의 목적에 적용)
- 예제 요구 사장
    -> 게임 아이템과 아이템 생성을 구현해주세요
    -> 아이템을 생성하기 전에 데이터 베이스에서 아이템 정보를 요청한다
    -> 아이템 생성후 복제 방지를 위해 데이터 베이스에 아이템 생성 정보를 남긴다
    -> 아이템 생성하는 주체를 ItemCreator로 이름을 짓는다
    -> 아이템은 item 이라는 interface로 다루 도록 한다
    -> 현재 아이템의 종류는 체력, 마력 회복 물약이 있다
- ex>
public interface Item {
    public void user();
}
```java
public abstract class ItemCreatoer {
    public Item create() {
        Item item;

        requestItesInfo();      //step1
        item = createItem();    //step2
        createItemLog();        //step3
        return item;
        // 이렇게 순서대로 만드는 것이 템플릿 메소드 패턴이다
    }
    abstract protected void requestItesInfo();
    abstract protected void createItemLog();
    abstract protected Item createItem();
    // 여기에 있는 함수 알고리즘이 변경이 되고 Main클래스의 코드는 바꿀 필요가 없다 -> 기능과 구현의 분리
}

public class HpPotion implements Item {
    @Override
    public void use(){
        System.out.println("체력회복")
    }
}
public class MpPotion implements Item {
    @Override
    public void use(){
        System.out.println("마력회복")
    }
}

public class HpCreator extends ItemCreatoer {
    @Override
    protected void requestItesInfo(){
        System.out.println("체력물약 정보를 가져온다")
    }
    @Override
    protected void createItemLog(){
        System.out.println("체력회복 물약을 새로 생성 했습니다")
    } 
    @Override
    protected Item reateItem(){
        return new HpPotion();
    }
}
public class MpCreator extends ItemCreatoer {
    @Override
    protected void requestItesInfo(){
        System.out.println("마력 물약 정보를 가져온다")
    }
    @Override
    protected void createItemLog(){
        System.out.println("마력 회복 물약을 새로 생성 했습니다")
    } 
    @Override
    protected Item reateItem(){
        return new HpPotion();
    }
}

public class Main {
    public static void main(String[] args){
        ItemCreatoer creator;
        Item item;
        creator = new HpCreator();
        item = creator.create(); // -> creator는 create말고 requestItesInfo, createItemLog 접근을 막는다
        item.use();
        
        creator = new MpCreator();
        item = creator.create(); // -> create
        item.use();
    }
}
```
*GOF
1. 의도 
- 객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 클래스의 인스턴스를 생성할지에 대한 결정은 서브클래스가 내리도록 한다

2. 다른 이름
- 가상 생성자(virtual constructor)

3. 동기
- 프레임 워크는 추상 클래스를 사용하여 객체 간의 관련성을 정의하고 유지할 수 있고, 이들 객체를 생성할 책임을 가진다
- 프레임 워크는 이들 추상 클래스 간의 상호작용을 책임져서 전체 시스템의 기본 동작 방식을 정의한다
- 이들 추상 클래스를 상속하는 서브클래스에서 구체적인 행동을 정의함으로써 새로운 응용프로그램을 만든다

4. 활용성
- 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
- 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때
- 객체 생성의 책임을 몇 개의 보조 서브클래스 가운데 하나에게 위임하고, 어떤 서브 클래스가 위임자인지에 대한 정보를 국소화 시키고 싶을 때

5. 참여자
- Product : Product 클래스에 정의된 인터페이스를 실제로 구현한다
- ConcreteProduct : Product 클래스에 정의된 인터페이스를 실제로 구현한다
- Creator : Product 타입의 객체를 반환하는 팩토리 메서드를 선언한다
            Creator 클래스는 팩토리 메서드를 기본적으로 구현하는데, 이 구현에서는 ConcreteProduct객체를 반환 한다
            또한 Product 객체의 생성을 위해서 팩토리 메서드를 호출 한다
- ConcreteCreator : 팩토리 메서드를 재정의하여 ConcreteProduct 의 인스턴스를 반환한다

6. 결과
- 응용프로그램은 Product 클래스에 정의된 인터페이스와만 동작 하도록 코드가 만들어지기 때문에, 
  사용자가 정의한 언떤 ConcreteProduct 클래스가 와도 동작 할수 있게 된다
- 단점은, 사용자가 ConcreteProduct 객체 하나만 만들려 할 때에도 Creator 클래스를 서브 클래싱해야 할지도 모른다
- 서브클래스에 대한 훅(hook) 메서드를 제공한다
    -> 팩토리 메서드로 클래스 내부에서 객체를 생성하는 것이 객체를 직접 생성하는 것보다 훨씬 응용성이 높아진다
    -> 팩토리 메서드 패턴에서는 객체별로 서로 다른 버전을 제공하는 훅 기능을 서브클래스에 정의한다
- 병렬적인 클래스 계통을 연결하는 역할을 담당한다

7. 구현
- 1. Creator 클래스를 추상 클래스로 정의하고, 정의한 팩토리 메서드에 대한 구현은 제공하지 않는 경우
    -> 추상 클래스로 정의할 때는 구현을 제공한 서브클래스를 반드시 정의 해야 한다
    -> 이때 아직 예측할 수 없는 클래스들을 생성해야 한느 문제가 생긴다
- 2. Creator가 구체 클래스이고, 팩토리 메서드에 대한 기본 구현을 제공 하는 경우
    -> Creator가 팩토리 메서드를 사용하여 유연성을 보장할 수 있다
    -> 객체의 생성은 별도의 연산으로 분리하여, 이 연산을 서브클래스에서 재정의하게 한다 -> inflearn 예시
- 팩토리 메서드를 매겨변수화 한다
    -> 팩토리 메서드를 이용해서 여러 종류의 제품을 생성하는 방법도 있다
    -> 팩토피 메서드가 매개변수를 받아서 어떤 종류의 제품을 생성할지 식별하게 만드는 것이다
    -> 물론, 팩토리 메서드가 생성하는 모든 객체는 Product라는 인터페이스를 만족해야 한다

8. 예제 코드
```java
class Mazegame {
    public :
        Maze* CreateMaze();
        // 팩토리 메서드들
        virtual Maze* MakeMaze() const
        { return new Maze; }
        virtual Room* MakeRoom(int n ) const
        { return new Room(n); }
        virtual Wall* MakeWall() const
        { return new Wall; }
        virtual Door* MakeDoor(Room* r1, *Room* r2) const
        {return new Door(r1, r2); }
}

Maze* MazeGame::CreateMaze() {
    Maze* aMaze = MakeMaze();

    Room* r1 = MakeRoom(1);
    Room* r2 = MakeRoom(2);
    Door* theDoor = MakeDoor(r1,r2);

    aMaze->AddRoom(r1);
    aMaze->AddRoom(r2);

    return aMaze;
}

class BombedMazegame : public MazeGame {
    public :
        BombedMazeGame();
        virtual Wall* MakeWall() const
        { return new BombedWall; }
        virtual Room* MakeRoom(int n)const
        { return new RoomWithBomb(n); }
}
```