장식자(Decorator pattern)
* 최종 정리
- 서브 클래스를 이용해 객체를 동적으로 확장하는 방식
- 클래스를 써서 기능을 확장하는 방법
- 빌더 
    -> 부품들을 더해서 완선된 하나를 만드는 것
- 장식자
    -> 이미 완성된 객체에 추가를 해나가는 방식
- 즉, 아이템을 하나하나 장착해 나가는 방식이라고 생각하면 된다

* Inflearn
- '동적'으로 '책임(기능) 추가'가 필요할 때 사용 할 수 있다
- 구조
    -> Component : 실질적인 인스턴스를 컨트롤하는 역할
    -> ConcreteComponent : Compnent의 실질적인 인스턴스의 부분이며, 책임 주체의 역할
    -> Decorator : Component와 ConcreteComponent를 동일시 하도록 해주는 역할
    -> ConcreteDecorator : 실질적인 장식 인스턴스 및 정의이며, 추가된 책임의 주체 부분

- 예제
-> 에스프레소 : 커피의 기본
-> 아메리카노 : 에스프레소 + 물
-> 카페라떼 : 에소프레소 + 스팀 밀그
-> 헤이즐넛 : 아메리카노 + 헤이즐넛 시럽
-> 카페모카 : 카페라떼 + 초콜릿
-> 캬라멜 마끼야또 : 카페라떼 + 캬라멜 시럽


Beverage <--------------- Base
    |
    |
AbstAdding <------------- Espresso

```java
public interface IBeverage {
    int getTotalPrice();
}
public class Base implements IBeverage {
    @Override
    public int getTotalPrice(){
        return 0;
    }
}

abstract public class AbstAdding implements IBverage {
    private IBeverage base;
    public AbstAdding(IBeverage base ){
        super();
        this.base = base;
    }
    @Overrice
    public int getTotalPrice(){
        return base.getTotalPrice();
    }
    protected IBeverage getBase(){
        return base;
    }
}

public class Milk extends AbstAdding {
    public Milk(IBeverage meterial){
        super(meterial);
    }
    @Override
    public int getTotlaPrice(){
        return super.getTotalPrice() + 50; // -> base에 50원을 더하게 된다
    }
}
public class Espersso extends AbsAdding {
    public Espresso(IBeverage base){
        super(base);
    }
    @Override
    public int getTotalPrice(){
        return super.getTotalPrice() + getAddPrice() // -> base에 가격을 더하게 된다
    }
    private static int getAddPrice(){
        espressoCount +=1;
        int addPrice = 100;
        if(espressoCount > 1){
            addPrice = 70;
        }
        return addPrice;
    }
}

public class Main {
    public static void main(String[] args){

        IBeverage beverage = new Base();

        beverage = new Espresso(beverage);  // 에스프레소 추가
        beverage = new Mile(beverate); // 우유 추가
    }
}
```

* GOF
1. 의도
-> 객체에 동적으로 새로운 책임을 추가할 수 있게 한다
-> 서브클래스를 생성하는 것보다 융통성 있는 방법을 제공한다

2. 다른 이름 : 랩퍼(Wrapper)

3. 동기
- 전체 클래스에 새로운 기능을 추가할 필요는 없지만, 개별적인 객체에 새로운 책임을 추가할 필요가 있다
    -> 상속을 하는 것이 그다지 유용해 보이지 않을 경우

4. 활용성
- 동적으로 또한 투명하게, 다시 말해 다른 객체에 영향을 쥐 않고 개개의 객체에 대한 새로운 책임을 추가하기 위해 사용한다
- 제거될 수 있는 책임에 대해 사용한다
- 실제 상속으로 서브클래스를 게속 만든느 방법이 실질적이지 못할 때 사용 한다

5. 참여자
- Component : 동적으로 추가할 서비스를 가질 가능성이 있는 객체들에 대한 인터페이스
- ConcreateComponent : 추가적인 서비스가 실제로 정의도어야 할 필요가 있는 객체
- Decorator : 객체에 대한 참조자를 관리하면서 component에 정의된 인터페이스를 만족하는 인터페이스를 정의
- ConcreteDecorator : Component 에 새롭게 추가할 서비스를 실제로 구현하는 클래스

6. 결과
- 단순한 상속보다 설계의 융통성을 더 많이 증대 시킬 수 있다
    -> 객체에 새로운 행동을 추가할 수 있는 가장 효과적인 방법이다
- 클래스 계통의 상부측 클래스에 많은 기능이 누적되는 상황을 피할 수 있다
    -> 장식자 패턴은 책임 추가 작업에서 필요한 비용만 그때 지불하는 방법을 제공한다
    -> 지금 예상하지 ㅁ소한 특성들을 한꺼번에 다 개발하기 위해 고민하고 노력하기보다는 발견하지 못하고 누락된 서비스들을
       장식자 객체를 통해 지속적으로 추가할 수 있다
- 장식자와 해당 그 장식자의 구성요소가 동일한 것은 아니다
- 장식자를 사용함으로써 작은 규모의 객체들이 많이 생긴다