상태 (State Pattern)
* 최종 정리
-

* Inflearn
-  '상태'를 '객체'로 나타내고 '행동' 구현한다
```java
public class Light {
    protected LightState lightState;
    private LightState offState = new LightState(){
        @Override
        public void on(){
            println("on");
            lightState = onState;
        }
        @Override
        public void off(){
            pritnln("Nothing");
        }
    };
    private LightState onState = new LightState(){
        @Override
        public void on(){
            println("Nothing");
            lightState = offState;
        }
        @Override
        public void off(){
            pritnln("ON");
        }
    };

    public Light(){
        lightState = OffState;
    }

    public void on(){
        lightState.on();
    }
    public void off(){
        lightState.off();
    }

}
interface LightState {
    void on();
    void off();
}
public class Main{
    public static void main(String[] args){
        Light light = new Light();
        light.off();
        light.on();
    }
}
```

* GOF
1. 의도
- 객체의 내부 상태에 따라 스스로 행동을 변경할 수 있게 허거하는 패턴으로, 이렇게 하면 객체는 마치 자신의 클래스를 바꾸는 것처럼 보인다

2. 다른 이름
- 상태 표현 객체(Object for State)

3. 동기
- TCP 통신을 할때 TCP상태에 따라서 동작하고자 하는 행동이 있다

4. 활용성
- 객체의 행동이 상태에 따라 달라질 수 있고, 객체의 상태에 따라서 런타임에 행동이 바뀌어야 한다
- 어떤 연산에 그 객체의 상태에 따라 달라지는 다중 분기 조거니 처리가 너무 많이 들어 있을 때
  객체의 상태를 표현하기 위해 상태를 하나 이상의 나열형 상수로 정의해야 한다. 동일한 조건 문장들을 하나
  이상의 연산에 중복 정의해야 할 때도 잦다. 이때, 객체의 상태를 별도의 객체로 정의하면, 다른 객체들과
  상관없이 그 객체의 상태를 다양화시킬 수 있다

5. 참여자
- Context 
    - 사용자가 관심 있는 인터페이스를 정의한다. 객체의 현재 상태를 정의한 ConcreteState 서브클래스의 인스턴스를
      유지 관리 한다
- State
    - Context의 각 상태별로 필요한 행동을 캡슐화하여 인터페이스로 정의 한다
- ConcreteState(서브클래스들)
    - 각 서브 클래스들은 Context의 상태에 따라 처리되어야 할 실제 행동을 구현한다

6. 결과
- 상태에 따른 행동을 국소화하며, 서로 다른 상태에 대한 행동을 별도의 객체로 관리한다
    - 임의의 한 상태에 관련된 모든 행동을 하나의 객체로 모을 수 있다
    - 이 방법을 쓰지 않으면 내부 상태를 저장해 둘 변수를 선언하고, Context의 연산이 그 데이터를 명시적으로 점검해야 한다
    - if, switch로 상태를 처리 하지 않아도 된다
- 상태 전이를 명확하게 만든다
    - 객체가 자신의 현재 상태를 오직 내부 데이터 값으로만 정의하면, 상태 전이는 명확한 표현을 갖지 못한다
    - State 객체는 Context 객체가 일관되지 않은 상태가 되는 것을 막아준다
- 상태 객체는 공유될 수 있다
    - 상태는 단지 타입으로만 표현되므로, State객체는 인스턴스 변수 없이 여러 context클래스의 인스턴스롣
      객체를 공유 할 수 있다