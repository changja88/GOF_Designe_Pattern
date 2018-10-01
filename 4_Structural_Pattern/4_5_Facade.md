퍼사드(Facade pattern)
* 최종 정리

* Inflearn
- '복잡한 과정'을 '간단'하게 제공
- 파사드 클래스는 여러 package를 묶어서 클라이언트 한테 간단한 함수 하나로 원하는 것을 전달한다
- 여러 함수나 클래스 생성을 묶어서 메소드 하나로 클라인언트가 편히 사용할 수 있도록 묶어 주는것

```java
// Package1
class Facade { 
    private HelpSystem01 helpSystem01;
    private HelpSystem02 helpSystem02;
    private HelpSystem03 helpSystem03;

    public Facade(){ // 해당 시스템들을 한번에 생성 해준다
        helpSystem01 = new HelpSystem01();
        helpSystem02 = new HelpSystem02();
        helpSystem03 = new HelpSystem03();
    }
    public void process(){ // 클라이언트가 이걸 하나 실행 하면 3개가 한번에 실행된다
        helpSystem01.process();
        helpSystem02.process();
        helpSystem03.process();
    }
}
// Package1
class HelpSystem01 { // 같은 package에서만 접근 가능
        public HelpySystem(){
        print("HELP !!");
    }
    public void process(){
        print("PROCESS !!");
    }
}
// Package1
class HelpSystem02 { // 같은 package에서만 접근 가능
        public HelpySystem(){
        print("HELP !!");
    }
    public void process(){
        print("PROCESS !!");
    }
}
// Package1
class HelpSystem03 { // 같은 package에서만 접근 가능
        public HelpySystem(){
        print("HELP !!");
    }
    public void process(){
        print("PROCESS !!");
    }
}

// package2
public class Main {
    public static void main(String[] args){
        Facade facade = new Facade();
        facade.process();

        HelpSystem1 // -> 접근 안됨 (package안에서만 접근 할 수 있도록 막아놨기 때문)
    }
}
```

* GOF
1. 의도
-> 한 서브시스템 내의 인터페이스 집합에 대한 획일화된 하나의 인터페이스를 제공하는 패턴
-> 서브시스템을 사용하기 쉽도록 상위 수준의 인터페이스를 정의한다

2. 동기
- 시스템을 서브시스템으로 구조화 하면 복잡성을 줄이는데 큰 도움이 된다
    -> 서스시스템들 사이의 의사소통 및 족송성을 최소화 하면서 해야된다
    -> 이걸 퍼사드 클래스가 가능하게 해준다
    -> 즉 퍼사드는 서브시스템의 일반적인 기능에 대한 단순화된 하나의 인터페이스를 제공한다
    -> 사용자가 로우 레벨에서 뭐가 일어나는지는 알 필요없이 서브시스템을 사용하도록 도와준다

3. 활용성
- 시스템 범위가 확장되면, 또한 구체적으로 설계되면 서브시스템은 계속 복잡해지고, 패턴을 적용하고 확장성을 고려하면서 만들기 때문에
  작은 클래스들이 많이 생기게 된다 
    -> 이걸 퍼사드가 해결해준다
- 서브 시스템과 사용자 코드 간의 결합도를 약하게 만든다
    -> 서브시스템내 정의된 요소들은 강하게 결합될수 있고, 서브시스템과 사용자 간의 결합이 약해지면 서브시스템 내의 요소를 다양화하는
       작업을 원활하게 할 수 있다
       