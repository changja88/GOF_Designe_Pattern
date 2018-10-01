추상 팩토리(Abstract Factory) 
* 최종 정리
-> Factory group 에서 원하는 Factory 를 뽑아내고 그 팩토리를 이용하여 그 팩토리가 만들수 있는 product를 만들어 내는 방식
    Factory Group                       |--- product
      AFactory     ----->  Factory -----|--- product
      A'Factory                         |___ product 
  유사한 제품을 만드는 펙토리 집합  
                          선택된 팩토리     선택된 팩토리를 이용하여 그 팩토리가 만들수 있는 product를 찍어낸다
- Builder 와 차이점
-> 복잡한 객체를 생성해야 할 때 추상 팩토리 패턴은 빌더 패턴과 비슷한 모습을 보인다.
   근본적인 차이가 있다면 빌더 패턴은 복잡한 객체의 단계별 생성에 중점을 둔 반면, 추상 팩토리 패턴은 제품의 유사군들이 존재할 때
   유연한 설계에 중점을 두는 것이다. 빌더 패턴은 생성의 마지막 단계에서 생성한 제품을 반환하는 반면, 추상 팩토리 패턴에서는 
   만드는 즉시 제품을 반환한다. 추상 팩토리 패턴에서 만드는 제품은 꼭 모여야만 의 있는 것이 아니라 하나만으로도 의미가 있다

- Factory Method 와의 차이
-> 추상 팩토리는 공장 자체를 만드는것 (공장은 부품들을 생성 할 수 있다)
   즉, 공장을 만들고 그 공장에서 부품을 계속 만들어야 한다
-> 추상 메소드는 공장을 만드는 것이 아니라 부품 하나를 만들어 내는 것, 즉 만드는 순간 끝이 난다


* Inflearn
- 관련 있는 객체의 생성을 가상화 할 수 있다
- 생성하는 부분을 가상화 해서 구체적인 부분을 가려주고, 클라이언트는 가상화 된 부분을 가지고 객체를 생성한다
- ex> 
-------------------------한 파일 안에 있어야 한다 -------------------------------
```java
// concreate class 집합
public class FactoryInstance { // 이게 concrete Factory를 찍어내는 abstract Factory를 만들어 내는 곳이다
    public static GuiFac getGuiFac(){
        swtich(){
            case 0: return new MacGuiFac();
            case 1: return new LinuxGuiFac();
            case 2: return new WinGuiFac();
        }
        return null
    }
}
public class LinuxGuiFac implements GuiFac { // 이게 'concrete' 팩토리다
    @Override
    public Button createButton(){
        return new LinuxButton();
    }
    @Override
    public TextArea createTextArea(){
        return new LinuxTextArea();
    }
}
public class LinuxButton implements Button {
    @Override
    public void click(){
        System.out.println("리눅스 버튼");
    }
}
public class LinuxTextArea implements TextArea{
    @Override
    publci String getText(){
        return "리눅스 텍스트에어리어";
    }
}
public interface GuiFac { // 이게 'abstract' 팩토리다
    public Button createButton();
    public TextArea createTextArea();
}
public interface Button {
    public void click();
}
public interface TextArea {
    public String getText();
}
------------------------------------------------------------------------
publci class Main(){
    public static void main(String[] args){
        GuiFac fac = new LinuxGuiFac(); // 이렇게 하게되면 망하는 것
        GuiFac fac = FactoryInstance.getGuiFac(); // 이렇게 해야 abstractFactory 리가 완성

        Button button = fac.createButton();
        TextArea textArea = fac.createTextArea();

        button.click();  -> "리눅스 버튼"
        System.out.println(area.getText()); -> "리눅스 텍스트에어리어"
    }
}
```
- 나의 이해 
- 목적 : 커스텀된 기능을 만들수 있는 추상 팩토리(서로 비슷한 객체를 만든다)들의 집합을 만들고 싶을 때 사용한다.
        추상 팩토리들은 컴스텀된 기능(최상위 기능으로 부터 커스텀된 기능)을 만들수 있느 객체를 만들 수 있다
        즉, abstrac Factory 한테 car Factory를 달라고 할수도, bike Factory를 달라고 할 수도 있다
- 방식 : interface 를 확용해여 최상위 기능을 만들고 하위 인터페이스들이 이를 구현하여 커스텀된 기능을 만들도록 한다 
        즉, interface로 커스텀된 기능들을 묶어놓는다(같은 타입이 되도록)


* GOF
- 1. 의도->상세화된 서브클래스를 정의하지 않고도 서로 관련성이 있거나 독립적인 여러 객체의 군을 생성하기 위한 인터페이스를 제공한다
- 2. 다른 이름->키드(Kit) 
- 3. 동기->몰라
- 4. 활용성
    - 객체가 생성되거나 구성, 표현 되는 방식과 무관하게 시스템을 독립적으로 만들고자 할 때
    - 여러 제품군 중 하나를 선택해서 시스템을 설정해야 하고 한번 구성한 제품을 다른 것으로 대체 할 수 있을 때
      이 부분에 대한 제약이 와부에도 지켜지도록 하고 싶을 때
    - 제품에 대한 클래스 라이브러리를 제공하고,
     - 관련 제품 객체들이 홤께 사용되도록 설계되었고, 그들의 구현이 아닌 인터페이스를 노출시키고 싶을 때
- 5. 참여자
    - AbstractFactory : 개념적 제품에 대한 객체를 생성하는 연산으로 인터페이스를 정의한다
    - ConcreteFactory : 구체적인 제품에 대한 객체를 생성하는 연산을 구현 합니다
    - ConcreteProduct : 구체적으로 팩토리가 생성할 객체를 정의하고,AbstractProduct가 정의하는 인터페이스를 구현합니다
    - Client : AbstractFactory, AbstractProduct 클래스에 선언된 인터페이스를 사용합니다
- 6. 협력방법
    - 일반적으로 ConcreteFactory클래스의 인스턴스 한 개가 런타임에 만들어집니다.이 ConcreteFactory는 어떤 특정 구현을 갖는 제품 객체를 생성합니다.
      서로 다른 제품 객체를 생성하려면 사용자는 서로 다른 구체 팩토리를 사용해야 합니다
    - AbstractFactory는 필요한 제품 객체를 생성하는 책임을 ConcreteFactory서브 클래스에 위임합니다
- 7. 결과
    -구체적인 클래스를 분리합니다
    - 추상 팩토리 패턴을 쓰면 응용프로그램이 생성할 객체의 클래스를 제어할 수 있다
    - 팩토리는 제품 객체를 생성하는 과정과 책임을 캡슐화한 것이기 때문에,구체적인 구현 클래스가 사용자에게서 분리된다
      ->제품군을 쉽게 대체할 수 있도록 합니다
    - 추상 팩토리는 필요한 모든 것을 생성하기 때문에 전체 제품군은 한번에 변경이 가능하다->제품 사이의 일관성을 증진시킨다
    - 응용프로그램은 한 번에 오직 한 군데에서 만든 객체를 사용하도록 함으로써 프로그램의 일관성을 갖도록 한다->새로운 종류의 제품을 제공하기 어렵다
    - 새로운 종류의 제품을 만들기 위해 기존 추상 팩토리를 확장하기가 쉽지 않다
    - 생성되는 제품이 추상 팩토리가 생성할 수 있는 제품 집합에만 고정되어 있기 때문이다
    - 새로운 종류의 제품이 등장하면 팩토리의 구현을 변경해야 한다
- 8. 구현
     - 1>팩토리를 단일체로 정의한다
        ->전형적으로 응용프로그램은 한 제품군에 대해서 하나의 ConcreteFactory 인스턴스만 있으면 된다
          즉,갖가지 제품의 종류를 만들어 내는 팩토리는 제품군에 대해서 하나면 되는 것이다
          즉, 단일체로 구현이 바람직하다, 단일체 역시 생성 패턴의 한 종류이다
    - 2 > 제품을 생성한다->AbstractFactory는 단지 제품을 생성하기 위한 인터페이스를 선언하는 것이고 그것을 생성하는
          책임은 product의 서브클래스인 ConcreteProduct안에 있다.이를 위한 가장 공통적인 방법은 각 제품을 위해서
          팩토리 메서드를 정의하는 것이다 AbstractFactory는 각 제품 생성을 위한 팩토리 메서드를 overriding 함으로써
          각 제품의 인스턴스를 만든다 각 제품이 약간 다르다면 각 제품군을 위한 새로운 ConcreteFactory 서브클래스가 필요하다

 - 예제 코드
class MazeFactory{
    public:
        MazeFactory();
        virtual Maze *MakeMaze() const
        { return new Maze; }
        virtual Wall *MakeWall() const
        { return new Wall; }
        virtual Room *MakeRoom() const
        { return new Room(n); }
        virtual Door *MakeDoor(Room *r1, Room *r2) const
        { return new Door(r1, r2); }
}
//  - MazeFactory를 매개변수로 받도록 하여 
Maze * MazeGame:: Create(MazeFactory & factory){
    Maze* aMaze = factory.MakeMaze();
    Room* r1 = factory.MakeRoom(1);
    Room* r2 = factory.MakeRoom(2);
    Door* Door = factory.MakeDoor(r1,r2);

    aMaze->AddRoom(r1);
    aMaze->AddRoom(r2);

    r1->SetSide(North, factory.MakeWall())
    r2->SetSide(North, factory.Ma)
}

class EnchantedMazeFactory : public MazeFactory {
    // MazeFactory를 상속받아 부모 클래스에 정의된 연산을 재정의 한후, 구체적인 요소를 생성하여 반환하도록 구현하는 서브클래스가
    public:
        EnchantedMazeFactory();
        virtual Room* MakeRoom(int n) const
        { return new EnchantedRoom(n, CastSpell()); }
        // 이때는 Room을 상속받은 EnchantedRoom 의 인스턴스를 생성하여 반환함

        virtual Door* MakeDoor(Room* r1, Room* r2) const
        { return new DoorNeedingSpell(r1, re1); }
        // 이 때는 Door를 상속받은 DoorNeddingSpell의 인스턴스를 생성하여 반환함
}

class StandardMazeBuilder : public MazeBuilder {
    public:
        virtual void BuildMaze();
        virtual void BuildRoom(int);
        virtual void BuildDoor(int,int);

        virtual Maze* GetMaze();
    private:
        Direction CommonWall(Room*, Room*);
        Maze* _currentMaze;
};