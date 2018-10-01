빌터 패턴(Builder)
* 최종 정리
-> 여러가지 부품들을 주워 모아다가 Builder를 이용해서 부품들을 합체 시켜서 product를 만들어 내는 방식
    Parts
      A             
      B    -----> Builder -----> Product
      C
    별상관 없는 부품 A,B,C가 있다
                  전달 받은 부품을 보아서 Product를 만들어 낸다


-> 복잡한 객체를 생성해야 할 때 추상 팩토리 패턴은 빌더 패턴과 비슷한 모습을 보인다.
   근본적인 차이가 있다면 빌더 패턴은 복잡한 객체의 단계별 생성에 중점을 둔 반면, 추상 팩토리 패턴은 제품의 유사군들이 존재할 때
   유연한 설계에 중점을 두는 것이다. 빌더 패턴은 생성의 마지막 단계에서 생성한 제품을 반환하는 반면, 추상 팩토리 패턴에서는 
   만드는 즉시 제품을 반환한다. 추상 팩토리 패턴에서 만드는 제품은 꼭 모여야만 의 있는 것이 아니라 하나만으로도 의미가 있다
   
* Inflearn
- '복잡한 단계'가 필요한 인스턴스 생성을 빌더 패턴을 통해서 구현 할 수 있다. 
- 복잡한 단계를 거쳐야 생성되는 객체의 구현을 서브 클래스에게 넘겨주는 패턴
- ex> 1
```java
public class Main {
    public static void main(String[] args){
        Computer computer = new Computer("i7", "16GB", "256SSD");
        //  컴퓨터 만드는데 부품이 한 200개 필요하다고 하면 이걸 client가 하나하나 입력하는게 미친짓이 될수도 있다

        ComputerFactory factory = new ComputerFactory();
        factory.setBlueprint(new LgGramBlueprint());
        Computer computer = factory.make().getComputer();

        factory.setBlueprint(new MacBlueprint());
        Computer computer = factory.makeAndGet();
    }
}
public class Computer {
    super();
    private String cpu;
    private String ram;
    private String storage;

    public String getCpu(){
        return cpu;
    }
    public void setCpu(String cpu){
        this.cpu = cpu;
    }
    public String getRam(){
        return ram;
    }
    public void setCpu(String ram){
        this.ram = ram;
    }
    public String getCpu(){
        return storage;
    }
    public void setCpu(String storage){
        this.cstoragepu = storage;
    }
}
public abstract class BluePrint { // Abstract Builder
    abstract public void setCpu();
    abstract public void setRam();
    abstract public void setStorage();
    abstract public void getComputer();
}

public class LgGramBlueprint implement BluePrint { // Concrete Builder
    private Computer computer;
    
    public LgGramBlueprint(){
        computer = new Computer("default", "default", "default");
    }

    @Override
    public void setCpu() {
        computer.setCpu("i7");
    }
    @Override
    public void setRam() {
        computer.setRam("8g");
    }
    @Override
    public void setStorage() {
        computer.setStorage("256g ssd");
    }
    @Override
    public Compute getComputer(){
        return computer;
    } 
}

public class ComputerFactory {
    private BluePrint bluePrint;

    public setBlueprint(BluePrint bluePrint){
        this.BluePrint = BluePrint
    }
    public void make(){
        bluePrint.setCpu;
        bluePrint.setRam;
        bluePrint.setStorage;
    }
    public Compute getComputer(){
        return bluePrint.getComputer();
    }
}
```
- ex> 2
```java
public class Main {
    public static void main(String[] args){
        Computet computer = new Computer("255g ssd", "i7", "8g"); // 이런 실수를 할 수 있다

        Computer computer = ComputeBuilder
          .start()
          .setCpu("i7")
          .setRam("8g")     // 셋중에 하나 안해도됨 기본적으로 default가 들어가 있기 때문
          .setStorage("256 ssd")
          .build();
    }
}

public class Computer {
    super();
    private String cpu;
    private String ram;
    private String storage;

    public String getCpu(){
        return cpu;
    }
    public void setCpu(String cpu){
        this.cpu = cpu;
    }
    public String getRam(){
        return ram;
    }
    public void setCpu(String ram){
        this.ram = ram;
    }
    public String getCpu(){
        return storage;
    }
    public void setCpu(String Storage){
        this.cstoragepu = storage;
    }
}

public class ComputeBuilder {
    private Computer computer;
    private ComputeBuilder(){
        computer = new Computer("default", "default", "default");
    }
    public static ComputeBuilder start(){
        return new ComputeBuilder();
    }
    public ComputeBuilder setCpu(String string){
        computer.setCpu(string);
        return this;
    }
    public ComputeBuilder setRam(String string){
        computer.setRam(string)
        return this;
    }
    public ComputeBuilder setStorage(String string){
        computer.setStorage(string)
        return this;
    }
    public Computer build(){
        retrun this.computer;
    }
}
```



* GOF
1. 의도 : 복잡한 객체를 생성하는 방법과 표현하는 방법을 정의하는 클래스를 별도록 분리하여, 서로 다른 표현이라도 이를 생성 
        할 수 있는 동일한 절차를 제공할 수 있도록 하기위함

2. 동기
-> Text Formater를 만드는데 현재 ASCII, RTF로 변환하는 컨버터는 있지만 다른 컨버터도 추가될 가능성이 있다
-> 문서 포맷을 해석하는 알고리즘을 다른 형태로 어떻게 변환할 것인가를 결정하는 알고리즘과 분리 해낸다

3. 활용성 
-> 복합 객체의 생성 알고리즘이 이를 합성하는 요소 객체들이 무엇인지 이들의 조립 방법에 독립적일 때
-> 합성할 객체들의 표현이 서로 다르더라도 생성 절차에서 이를 지원해야 할 때

4. 참여자
- 빌더 : Product 객체의 일부 요소들을 생성하기 위한 추상 인터페이스를 정의한다
- Concrete Builder : 클래스에 정의된 인터페이스를 구현하며, 제품의 부품들을 모아 빌더를 복합한다. 생성한 요소의 표현을
                    정의하고 관리한다. 또한 제품을 검색하는 데 필요한 인터페이스를 제공한다
- Director : 빌더 인터페이스를 사용하는 객체를 합성한다
- Product : 생성할 복합 객체를 표현한다. Concrete Builder는 product의 내부 표현을 구축하고 복합 객체가 어떻게
            구성 되는지에 관한 절차를 정의한다

5. 협력방법
- 사용자는 Director 객체를 생성하고, 이렇게 생성한 객체를 자신이 원하는 Builder 객체로 합성해 나간다
- 제품의 일부가 구축 될 때마다 Director는 Builder에 통보합니다
- Builder는 Director의 요청을 처리하여 제품에 부품을 추가합니다
- 사용자는 Builder에서 제품을 검색합니다

6. 결과
- 제품에 대한 내부 표현을 다양하게 변화할 수 있다
- 생성과 표현에 필요한 코드를 분리 할 수 있다
- 복합 객체를 생성하는 절차를 좀더 세밀하게 나눌 수 있다

7. 구현
- 1. 조합과 구축에 필요한 인터페이스를 정의한다
    -> Builder는 단계별로 젶무들을 생성한다
    -> 이를 위해서 모든 종류의 제품을 생성하는 데 필요한 일반화된 연산을 정의한다
- 2. 제품에 대한 추상 클래스는 필요 없는가?
    -> 보통 제품간에 공통점이 없기 때문에 공통적인 기본 클래스를 준다고 해서 얻을게 없다
- 3. Builder에 있는 메서드에 대해서는 구현을 제공하지 않는게 일반적이다
    -> 구현부를 비워두어서 서브 클래스에서 재정의하는게 일반적이다

8 예제 코드
```java
class MazeBuilder {
    public:
        virtual void BuildMaze() { }
        virtual void BuildRoom(int room) { }
        virtual void BuildDoor(int roomFrom, int roomTo) { }
        
        virtual Maze* GetMaze() { return 0; }
    
    protected:
        MazeBuilder();
};
Maze* MazeGame::CreateMaze (MazeBuilder& builder) {
    builder.BuildMaze();

    builder.BuildRoom(1):
    builder.BuildRoom(1);
    builder.BuildDoor(1, 2);

    return builder.GetMaze();
}
-> 빌더 객체가 미로의 내부표현 (방, 문, 벽)을 은닉하고 있다(캡슐화).
-> 미로나 방, 문, 벽을 만드는 과정의 연속이지, 이들이 모여서 미로를 복합하는지, 방과 문의 관계가 어떻게 되는지는 전혀 알 수 없다
Maze* MazeGame::CreateComplexMaze (MazeBuilder& builder) {
    builder.BuildRoom(1);
    //...
    builder.BuildRoom(1001);
    return builder.GetMze();
}
```
-> 이렇게 엄청 복잡한 미로를 만들어도, 클라이언트는 안에서 뭐가 어떤 관계를 가지고 만들어 지는지 알수가 없다(캡슐화)
