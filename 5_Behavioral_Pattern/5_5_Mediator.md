중재자 (Mediator Pattern)
* 최종 정리
- Mediator를 가운데에다가 두고 복수개의 Colleague들로 부터 요청을 받는다
- Mediator는 받은 요청을 가지고 지가 따로 처리 할게 있으면 처리(사전처리)를 하고 요청을 보낸 Colleague한데 돌려 보낸다
- Colleague는 Medaitor 한테 지가 보낸 요청이 사전처리가 끝나고 돌아오면 이제 지가 할을을 한다

* Inflearn
- '복잡한 관계'를 간단한 관계로 구현한다
- M:N의 관계를 1:1 관계로 만든다
```java
public abstract class Colleague {
    private Mediator mediator;
    public boolean join(Mediator mediator){
        this.mediator = mediator;
        Mediator.addColleague(this);
    }
    public void sendData(String data){
        if(mediator != null){
            mediator.mediate(data);
        }
    }
    abstract public void handle(String data){
        
    }
}
public abstract class Mediator {
    private List<Colleague> colleagues;
    public boolean addColleague(Colleague colleague){
        if(colleagues != null){
            return colleagues.add(colleague);
        }
        return false;
    }
    public abstract void mediate(String data);
}

public class ChatMediator implements Mediator{
    @Override
    public void mediate(String data){
        for(Colleague colleague : colleagues){
            // 중재 가능성이 있는 정보
            colleague.handle(data);
        }
    }
}
public class ChatColleague implements Colleage {
    @Override
    public void handle(String data){
        println(this + data);
    }
}

public class Main {
    public void main(String[] args){
        Colleague colleague1 = new ChatCollague();
        Colleague colleague2 = new ChatCollague();
        Colleague colleague3 = new ChatCollague();

        Mediator mediator = new ChatMediator();

        colleague1.join(mediator); // 중재자 한데 등록
        colleague2.join(mediator);
        colleague3.join(mediator);

        colleague1.sendData("AAA"); // 이요청은 중재를 거쳐서 지한테 다시 돌아온다
        colleague2.sendData("AAA");
        colleague3.sendData("AAA");
    }
}
```

* GOF
1. 의도
- 한 집합에 속해있는 객체의 상호작용을 캡슐화하는 객체를 정의한다
- 객체들이 직접 서로를 참조하지 않도록 하여 객체 사이의 소결합을 촉진시키며, 개발자가 객체의 상호작용을 독립적으로 다양화 시킬 수 있게 한다

2. 동기
- 객체지향은 행동을 여러 객체게ㅔ 분산시켜 처리하도록 권고한다 -> 수많은 연결 관계가 객체 사이에 존재하게 되는 문제점 발생
    -> 중재자 객체를 활용하면 상호작용과 관련되 ㄴ행동을 하나의 객체로 모아서 이런 문제를 피해 갈 수 있다

3. 활용성
- 여러 객체가 잘 정의된 형태이기는 하지만 복잡한 상호작용을 가질 때. 객체간의 의존성이 구조화되지 않으며, 잘 이해하기 어려울 때
- 한 객체가 다른 객체를 너무 많이 참조하고, 너무 많은 의사소통을 수행해서 그 객체를 재상용하기 힘들 때
- 여러 클래스에 분산된 행동들이 상속 없이 상황에 맞게 수정 되어야 할 때

4. 참여자
- Mediator 
    - 객체와 교류하는 데 필요한 인터페이스를 정의한다
- ConcreteMediator
    - Colleague 객체와 조화를 이뤄서 협력 행동을 구현하며, 자신이 맡은 colleague 를 파악 하고 관리한다
- Colleague
    - 자신의 중재자 객체가 무엇인지 파악한다
    - 다른 객체와 통신이 필요하면 그 중재자를 통해 통신되도록 하는 동료 객체를 나타내는 클래스 이다

5. 결과
- 서브클래싱을 제한한다
    - 중재자는 다른 객체 사이에 분산된 객체의 행동들을 하나의 객체로 국한한다
    - 행동을 변경하고자 한다면 Mediator 클래스를 상속하는 서브 클래스만 만들면 된다
- Colleague 객체 사이의 종속성을 줄인다
    - 중재자는 행동에 참여하는 객체간의 소결합을 증진 시킨다
    - Mediator 클래스와 Colleague 클래스 각각을 독립적으로 다양화시킬 수 있고 재사용할 수 있다
- 객체 프로토콜을 단순화 한다
    - 중재자는 다 대 다의 관계를 일 대 다의 관계로 축소 시킨다
- 객체 간의 협력 방법을 추상화 한다
    - 객체 사이의 중재를 독립적인 개념으로 만들고 이것을 캡슐화함으로써, 사용자는 각 객체의 행동과 상관없이 객체간 연결 방법에만
      집중 할 수 있다
- 통제가 집중된다
    - 중재자 패턴은 상호작용의 복잡한 모든 것들이 자신 내부에서만 오가게 합니다.
    - 중재자 객체는 동료 객체 간의 상호작용에 관련된 프로토콜을 모두 캡슐화하기 때문에 복잡해진다