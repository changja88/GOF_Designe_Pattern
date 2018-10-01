* 원형 패턴(Prototype Pattern)
* 최종 정리

* Inflearn
- 프로토 타입 패턴을 통해서 '복잡한 인스턴스를 복사'할 수 있다
- 생상 비용이 높은 인스턴스를 복사를 통해서 쉽게 생산하는 패턴
    -> 종류가 너무 많아서 클래스로 정리되지 않은 경우
    -> 클래스로 부터 인스턴스 생성이 어려운 경우
- 예제 요구 사항
    - 일러스트레이터와 같은 그림 그리기 툴을 개발 중이다. 어떤 모양을 그릴수 있도록 하고 복사 붙여넣기 기능을 구혀해라
    - 복사 후 붙여 넣기를 하면 두 도형이 겹치는데 안겹치도록 살짝 옆으로 이동하게 해주세요
- ex>
```java
public class shape implements Clonable {
    // java 에서는 clonable을 지원 한다
    private String id;
    public void setId(String id){
        this.id = id;
    }
    public String getId(){
        return id;
    }
}
private class Circle extends Shape {
    private int x,y,r;

    public Circle(int x, int y, int r){
        super();
        this.x = x;
        this.y = y;
        this.r = r;
    }

    private Circle copy() throws CloneNotSupportedException {
        Circle circle = (Circle)clone(); // 자바는 clonable을 지원 해주기 때문에 쉽게 할 수 있다
        circle.x +=1;
        circle.y +=1;
        return circle;
    }
}

public class Main {
    public static void main(String[] args){
        Circle circle1 = new Circle(1,1,3);
        Circle circle2 = circle1.copy();

        circle1.getX(); // -> 1
        circle2.getX(); // -> 1
    }
}
```
- ex> 깊은 복사, 얕은 복사
```java
public class Main {
    public static void main(String[] args){
        Cat navi = new Cat();
        navi.setName("navi");
        navi.setAge(new Age(2012, 3));

        Cat yo = navi; // 얕은 복사
        yo.setName("yo");
        yo.setAge(new Age(2013,2));
        // 여기에서 navi, yo 이름 출려하면 둘다 yo로 나옴 (주소 값이 같기 때문) -> 얕은 복사 

        Cay yo2 = navi.copy();
        yo2.setName("yo");
        yo2.setAge(new Age(2013, 2));
        // 여기에서 navi, yo 이름을 출력하면 navi, yo로 나온다 -> 깊은 복사


    }
}
public class Cat implements Clonable {
    private String name;
    private Age age;

    public void setName(String name){
        this.name = name
    }
    public void getName(){
        return name;
    }
    public void setAge(Age age){
        this.age = age
    }
    public void getAge(){
        return age;
    }
    private Cat copy() throws CloneNotSupportedException {
       Cat ret = (Cat) this. clone();
       ret.setAge(new Age(this.getYear(), this.getValue()));
       return ret;
    }
}
public class Age {
    int year;
    int vlaue;

    public Age(int, year, int value){
        super();
        this.yet = year;
        this.value = value;
    }
    //getter, setter 있다고 쳐
}
```

* GOF
1. 의도 : 원형이 되는 인스턴스를 사용하여 생성할 객체의 종류를 명시하고, 이렇게 만든 견본을 복사해서 새로운 객체를 생성한다

2. 동기 
-> 원형 패턴을 사용하면 심지어 클래스 수를 더 줄일 수도 있다

3. 활용성
- 원형 패턴은 제품의 생성, 복합, 표현 방법에 독립적인 제품을 만들고자 할 때 쓰인다
- 인스턴스화할 클래스를 런타임에 지정할 때 (동적 로딩 등등)
- 제품 클래스 계통과 병렬적으로 만드는 팩토리 클래스를 피하고 싶을 때
- 클래스의 인스턴스들이 서로 다른 상태 조합 중에 어느 하나일 때 원형 패턴을 사용한다
    -> 원형을 미리 초기화 해두고, 나중에 이를 복제해서 사용한다 (매번 필요한 상태 좋바의 값들을 수동적으로 초기화 하는 것보다 편하다)

4. 참여자
- Prototype : 자신을 복제 하는데 필요한 인터페이스를 정의한다
- ConcreatePrototype : 자신을 복제하는 연산을 구현한다
- Client : 원형에 자기 자신의 복제를 요청하여 새로운 객체를 생성한다

5. 협력방법
- 사용자는 원형 클래스에 스스로 복제하도록 요청한다

6. 결과
- 원형 패턴은 추상 팩토리 및 빌더와 비슷한 결과를 만든다. 사용자 쪽에는 어떤 구체적인 제품이 있는지 알리지 않아도 되기 때문에
  사용자 쪽에서 상대
- 런타임에 새로운 제품을 추가하고 삭제할 수 있다
    -> 원형 패턴을 이용하면 사용자에게 원형으로 생성되는 인스턴스를 등록하는 것만으로도 시스템에 새로운 제품 클래스를 추가 할 수 있다
    -> 런타임에 새로운 원형을 넣고 빼기가 쉽다는 점에서 다른 생성 패턴에 비해 유연성을 지니고 있다
- 값들을 다양화함으로써 새로운 객체를 명세한다
- 구조를 다양화함으로써 새로운 객체를 명세할 수 있다
- 서브 클래스의 수를 줄일 수 있다
- 동적으로 클래스에 따라 응용프로그램을 설정할 수 있다

7. 구현
1. 원형 관리자를 사용한다
- 원형의 수가 아직 정해지지 않은 때라면 가능한 원형이 등록된 레지스트리를 관리해야 한다
- 사용자는 원형을 복제하기 전에 레지스트리에 원형이 있는지 먼저 알아 본다. 이런 레지스트리를 가리켜 '원형 관리자'라고 한다
2. Clone() 연산을 구현한다
- 복제 기능은 대부분의 언어에서 웬만큼은 지원을 한다
3. Clone()을 초기화 한다
- 원형 클래스의 상태를 (재)설정하기 위한 연산 을 재공 해준다 

8. 구현
```java
class MazePrototypeFactory : public MazeFactory {
    public :
        MazePrototypeFactory(Maze*, Wall*, Room*, Door*);

        virtual Maze* MakeMaze() const;
        virtual Room* MakeRoom(int) const;
        virtual Wall* MakeWall() const;
        virtual Door* MakeDoor(Room*, Room*) const;

    private :
        Maze* _prototypeMaze;
        Room* _prototypeMaze;
        Wall* _prototypeMaze;
        Door* _prototypeMaze;
};
MazePrototypeFactory:: MazeProtoypeFactory(
    Maze* m, Wall* w, Room* r, Door* d
) {
    _prototypeMaze = m;
    _prototypeRoom = w;
    _prototypeWall = r;
    _prototypeDoor = d;
}

Wall* MazePrototypeFactory:: MakeWall ()contst {
    return _prototypeWall->Clone();
}
Door* MazePrototypeFactory:: MakeDoor (Room* r1, Room* r2) const {
    Door* door = _prototypeDoor->Clon();
    door-> Initialize(r1, r2);
    return door;
}

MazeGame game;
MazePrototypeFactory simpleMazeFactory(
    new Maze, new Wall, new Room, new Door
);
// MazePrototypeFactory를 이용하여 미로를 만들고 있음

Maze* maze = game.CreateMaze(simpleMazeFactory);

// 미로 형식을 바꾸려면 MazePrototypeFactory를 이용하되 다른 원형으로 초기화 하면 된다
MazePrototypeFactory bombedMazeFactory (
    new Maze, new BoombedWall, new RoomWithBomb, new Door
);
Maze* maze = game.CreateMaze(bombedMazeFactory);



```
