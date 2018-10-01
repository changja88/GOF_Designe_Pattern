전략패턴 (Strategy Pattern)
* 최종 정리
- 클라이언트는 병법서를 보고 원하는 병법을 가져다 쓰면된다. 각각의 병법은 병법서가 제공한다
- 접근점을 통해서 병법을 바꿔 쓸 수 있도록 해준다


* Inflearn
- 여러가지 알고리즘의 하나의 '추상적인 접근점'을 만들어 접근점에서 서로 교화 가능하도록 하는 패턴
```java
public interface Weapon {
    public void attack();
}
public class Knife implements Weapon{
    @Override
    public void attack(){
        print("공격");
    }
}
public class Sword implements Weapon{
    @Override
    public void attack(){
        print("공격");
    }
}
public class GameCharacter {
    private Weapon weapon; // 접근점
    public void setWeapon(Weapon weapon){
        this.weapon = weapon;
    }
    public void attack(){
        // Delegate 패턴
        weapon.attack();
    }
}
```

* GOF
1. 의도
- 동일 계열의 알고리즘군을 정의하고, 각 알고리즘을 캡슐화 하며, 이들을 '상호교환'이 가능하도록 만든다
- 알고리즘을 사용하는 클라이언트와 상관없이 독립적으로 알고리즘을 다양하게 변경할 수 있게 해준다

2. 다른이름
- 정책(policy)

3. 동기
- 캡슐화된 알고리즘들을 가리켜 '전략'이라고 한다
- 텍스트를 분석 하는데 A,B,C 전략을 골라가면서 사용한다

4. 활용성
- 행동들이 조금씩 다를 뿐 개념적으로 관련된 많은 클래스들이 존재할 때.
  전략 패턴은 많은 행동 중 하나를 가진 클래스를 구성할 수 있는 방법을 제공한다
- 알고리즘의 변형이 필요할 때. 이러한 변형물들이 알고리즘의 상속 관계로 구현될 때 전략 패턴을 사용 할 수 있다
- 사용자가 몰라야 하는 데이터를 사용하는 알고리즘이 있을 때. 노출 하지 많아야 할 복잡한 자료 구조는 Strategy 클래스에만 두면 된다

5. 참여자
- Strategy
    - 제공하는 모든 알고리즘에 대한 공통의 연산들을 인터페이스로 정의 한다
    - Context 클래스는 ConcreteStrategy 클래스에 정의한 인터페이스를 통해서 실제 알고리즘을 사용한다
- ConcreteStrategy
    - Strategy 인터페이스를 실제 알고리즘으로 구현한다
- Context
    - ConcreteStrategy 객체를 통해 구성된다.
    - 즉, Strategy객체에 대한 참조자를 관리하고, 실제로는 Strategy 서브클래스의 인스턴스를 갖고 있음으로써 구체화 한다
    - 또한 Strategy 객체가 자료에 접근해가는 데 필요한 인터페이스를 정의한다

6. 결과
- 동일한 계열의 관련 알고리즘군이 생긴다
- 서브클래싱을 사용하지 않는 대안이다
    - Strategy 클래스로 독립시키면 Context와 무관하게 알고리즘을 변형시킬 수 있고, 알고리즘을 바꾸거나 이해하거나 확장 하기 쉽다
- 조건문을 없앨 수 있다
    - 원하는 행동들을 선택하는 조건문을 없앨 수 있다
    - 서로 다른 행동이 하나로 묶이면 조건문을 사용해서 정확한 행동을 선택할 수밖에 없다
      그러나 Stategy 클래스의 행동을 캡슐화하면 이들 조건문을 없앨 수 있다
- 구현의 선택이 가능해진다
    - 동일한 행동에 대해서 서로 다른 구현을 제공할 수 있다
    - 사용자는 서로 다른 시간과 공간이 필요한 여러 Strategy들 중 하나를 선택할 수 있게 한다
- 사용자는 서로 다른 전략을 알아야한다
    - 사용자는 적당한 전략을 선택하기 전에 전략들이 어떻게 다른지 이해해야 한다
- Strategy 객체와 Context 객체 사이에 의사소통 오버헤드가 있다
    - 서브클래스에서 구현할 알고리즘의 복잡함과는 상관없이 모든 ConcreteStrategy 클래스는 Strategy 인터페이스를 공유한다
      따라서 어떤 ConcreteStrategy 클래스는 이 인터페이스를 통해 들어온 모든 정보가 필요하지 않은데 떠안아야 할 때가 생긴다
- 객체 수가 증가한다
    - 