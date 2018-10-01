메멘토 (Memento Pattern)
* 최종 정리
- 객체를 만들 때 그 상태를 저장해 놓을 객체를 동반해서 만들어 버린다
- 원본(Originator) 상태를 복사본에(Memento) 저장해놓는 것

* Inflearn
- 객체의 상태를 저장하고 이전상태로 복구한다
```java
public class Originator {
    // 상태를 가지고 있음
    String state; // 저장하고 싶은 것

    public Memento createMemento() {
        return new Memento(state);
    }
    public void restoreMemento(Memento memento){
        this.sate = memento.getState();
    }
    public String getState(){
        return state;
    }
    public void setSate(String state){
        this.state = state
    }
}

public class Memento {
    // Originator의 상태를 저장 해주는 객체
    String state;
    protected Memento(String state){
        this.state = state;
    }
    protected String getState(){
        return this.state;
    }
    // protected로 해서 외부에서 Memento 상태 값을 바꾸지 못하도록 한다
}

public class Main {
    public static void main(String[] args){
        Originator originator = new Originator();
        Stack<Memento> mementos = new Stakc<>(); //care taker

        originator.setState("state 1");
        mementos.push(originator.createMemento());
        originator.setState("state 2");
        mementos.push(originator.createMemento());
        originator.setState("state 3");
        mementos.push(originator.createMemento());

        orginator.restoreMemento(mementos.pop()); // ->state3
        orginator.restoreMemento(mementos.pop()); // ->state2
        orginator.restoreMemento(mementos.pop()); // ->state1
    }
}
```

* GOF
1. 의도 
- 캡슐화를 위배하지 않은 채 어떤 객체의 내부 상태를 잡아내고 실체회 시켜 둠으로써, 이후 해당 객체가 그 상태로 되돌아올 수 있도록 한다

2. 다름 이름
- 토큰(Token)

3. 동기
- 체크포인트를 구현할 때나 오류를 복구하거나 연산 수행 결과를 취소하는 메커니즘을 구현하려면 내부 상태 기록이 필요한다
- 객체는 자체적으로 상태의 일부나 전부를 캡슐화하여 상태를 외부에 공개하지 않기 때문에, 다른 객체는 상태에 접근 하지 못한다
- 원조본(Originator)객체가 가진 내부 상태의 스냅샷을 저장하는 객체이다

4. 활요성
- 어떤 객체의 상태에 대한 스냅샷을 저장한 후 나주에 이상태로 복구 해야 할 때
- 상태를 얻는 데 피룡한 직접적인 인터페이스를 두면 그 객체의 구현 세부사항이 드러날 수밖에 없고, 이것으로 객체의 캡슐화가 깨질 때

5. 참여자
- Memento(SolverState)
    - 원조본 객체의 내부 상태를 저장한다
    - 원조본 객체의 내부 상태를 필요한 만큼 저장해 둔다
    - 원조본 객체를 제외한 다른 객체는 자신에게 접근할 수 없도록 막는다
- Originator(ConstraintSolver)
    - 원조본 객체
    - 메멘토를 생성하여 현재 객체의 상태를 저장하고 메멘토를 사용하여 내부 상태를 복원한다
- Caretaker(보관자, 실행 취소 메커니즘)
    - 메멘토의 본관을 책임지는 보관자이다
    - 메멘토의 내용을 검사하거나 그 내용을 건드리지는 않는다
    - 원조본 객체에 메멘토 객체를 요청한다. 요청한 시간을 저장하며, 받은 메멘토 객체를 다시 원조본 객체게에 돌려준다(복구할경우)

6. 결과
- 캡슐화된 경계를 유지할 수 있다
    - 원조본만 메멘토를 다룰 수 있기 때문에 메멘토가 외부에 노출되지 않는다
- Originator 클래스를 단순화할 수 있다
    - 상태를 변경할 때마다 원조본을 갱신 해주기만 하면된다
- 메멘토의 사용으로 더 많은 비용이 들어 갈 수도 있다
- 제한 범위 인터페이스와 광범위한 인터페이스를 정의해야한다
- 메멘토를 관리하는데 필요한 비용이 숨어 있다
    - 보관자 객체는 자신이 보관하는 메멘토를 삭제할 책임이 있따