방문자 패턴 (Vistor Pattern)
* 최종 정리
- Vistor 가 Visitable을 돌아 다니면서 vistable에게 영향을 받게 된다

* Inflearn
- '객체'에서 '처리'를 '분리'해서 사용할 수 있다
- 객체에서 정의되지 못한 처리로직을 객체 밖에서 처리 한다
```java
public interface Vistor {
    public void visit(Visitable visitable);
}
public interface Visitable {
    public void accept(Vistor vsitor){
    }
}
public class VistorA implements Vistor{
    private int ageSum;
    public VisitorA(){
        ageSum = 0;
    }
    @Override
    public void visit(Visitable visitable){
        if(visitable instanceOf VisitableA){
            ageSum += ((VisitableA)visitable).getAge();
        }else{
        }
    }
}
public class VistableA implements Visitable {
    private int age;
    public VisitableA(int age){
        this.age = age;
    }
    @Override
    public void accpet(Vistor vistor){
        visitor.visist(this);
    }
}
public class Main {
    public static void main(String[] args){
        VisitableA v1 = new VisitableA();

        ArrayList<Visitable> visitables = new ArrayList<Visitable>();
        visitables.add(new VisitableA(1));
        visitables.add(new VisitableA(2));
        visitables.add(new VisitableA(3));
        visitables.add(new VisitableA(4));

        Visitor visitor = new VisitorA();
        for(Visitable visitable : visitiables){
            visitable.accept(visitor)
        }
        visitor.getAgeSum();  // -> 1+2+3+4 = 10이나옴
    }
}
```
* GOF
1. 의도
- 객체 구조를 이루는 원소에 대해 수행할 연산을 표현한다
- 연산을 적용할 원소의 클래스를 변경하지 않고도 새로운 연산을 정의할 수 있게 한다

2. 동기
- 방문자 패턴을 사용하면 두 개의 클래스 계통이 정의 된다
  하나는 연산이 적용되는 원소에 대한 클래스 계통, 또 하는 그 원소에 대해 적용할 연산을 정의하는 방문자 클래스 계통(Vistor)
- 새로운 연산을 추가하려면 방문자 클래스 계통에 새로운 서브클래스를 추가하면 된다

3. 활용성
- 다른 인터페이스를 가진 클래스가 객체 구조에 포함되어 있으며, 구체 클래스에 따라 달라진 연산을 이들 클래스의 객체에 대해 수행 하고자 할때
- 각각 특징이 있고, 관련되지 않은 많은 연산이 한 객체 구조에 속해 있는 개체들에 대해 수행될 필요가 있으며
  연산을 하나의 클래스 안에다 정의해 놓음으로써 관련된 연산이 함께 있을 수 있게 해준다
- 객체 구조를 정의한 클래스는 거의 변하지 않지만, 전체 구조에 걸쳐 새로운 연산을 추가하고 싶을 때 객체 구조를 변경하려면
  모든 방문자에 대한 인터페이스를 재정의해야 하는데, 이 작업에 잠재된 비용이 클 수 있다. 객체 구조가 자주 변경 될 때는
  해당 연산을 클래스에 정의하는 편이 더 낫다

4. 참여자
- Vistor
    - 객체 구조 내에 있는 각 ConcreteElement 클래스를 위한 Visit()연산을 선언한다 
    - 연산의 이름과 인터페이스 형태는 Visit()요청을 방문자에게 보내는 클래스를 식별 한다
    - 이로써 방문자는 방문된 원소의 구체 클래스를 결정할 수 있다
    - 그리고 나서 방문자는 그 원소가 제공하는 인터페이스를 통해 원소에 직접 접근할 수 있다
- ConcreteVistor
    - Vistor클랫에 선언된 연산을 구현한다
- Element
    - 방문자를 인자로 받아들이는 Accept() 연산을 정의 한다
- ConcreteElement
    - 인자로 방문자 객체를 받아들이는 Accept()연산을 구현 한다
- ObjectStructure
    - 객체 구조 내의 원소들을 나열할 수 있다
    - 방문자가 이 원소에 접근하게 하는 상위 수준 인터페이스를 제공한다

5. 결과
- Visotor 클래스는 새로운 연산을 쉽게 추가한다
- 방문자를 통해 관련된 연산들을 한 군데로 모으고 관련되지 않은 연산을 떼어낼 수 있다
- 새로운 ConcreteElement 클래스를 추가하기가 어렵다
- 클래스 계층구조에 걸쳐서 방문한다
- 상태를 누적할 수 있다
- 데이터 은닉을 깰 수 있다


