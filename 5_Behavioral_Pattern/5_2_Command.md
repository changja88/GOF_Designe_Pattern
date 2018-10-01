명령 연쇄 (Command Pattern)
* 최종 정리
- 명령을 캡슐화 하여 객체로 만든다
- 이 패턴의 핵심은 Command 추상 클래스 와 그안에 있는 Execute() 이다

* Inflearn
- '명력'을 '객체화' 할 수 있다
```java
public class Main {
    public static void main(String[] args){
        List<Command> list = new LinkedList<>();
        list.add(new Command()){
            @Override
            public void execute(){
                print("작업1");
            }
        }
        list.add(new Command()){
            @Override
            public void execute(){
                print("작업2");
            }
        }
        list.add(new Command()){
            @Override
            public void execute(){
                print("작업3");
            }
        }
    }
}
public Interface Command {
    void execute();
}
```

* GOF
1. 의도 
- 요청 자체를 캡슐화 하는 것
- 이를 통해 요청이 서로 다른 사용자를 매개변수로 만들고, 요청을 대기시키거나 로깅하며, 될도릴 수 있는 연산을 지원한다

2. 다른 이름
- 작동(Action)
- 트랜잭션(Transaction)

3. 동기
- 항상 그렇지는 않지만, 요청받은 연산이 무엇이며, 이를 처리할 객체가 누구인지에 대한 아무런 정보 없이 임의의 객체에 메시지를 보내야
  할 때가 간간이 있다
- 명령 패턴은 툴킷 객체가 요청 자체를 개체로 바꿈으로써 명시되지 않은 응용프로그램 객체의 요청을 처리 할 수 있도록 지원한다

4. 활용성
- 수행할 동작을 객체로 매개변수화하고자 할 때.
    -> 절차지향 프로그램에서는 이를 '콜백' 함수, 즉 어딘가 등록되었다가 나중에 호출되는 함수를 사용해서 이러한 매개변수를 표현 한다
    -> 명령 패턴은 콜밸을 객체지향 방식으로 나타낸 것이다
- 서로 다른 시간에 요청을 명시하고, 저장하며, 실행하고 싶을 때
- 실행 취소 기능을 지원하고 싶을 때
    -> Commnad의 Execute()연산은 상태를 저장 할 수 있다
    -> Unexcute() 연산을 Command 클래스의 인터페이스에 추가한다
- 시스템이 고장 났을 때 재적용이 가능하도록 변경 과정에 대한 로깅을 지원하고 싶을 때
    -> Command 인터페이스를 확장 해서 load() 와 store() 연산을 정의하면 상태의 변화를 지속적으로 저장소에 저장 할 수 있다
- 기본적인 연산의 조합으로 만든 상위 수준 연산을 써서 시스템을 구조화 하고 싶을 때
    -> 정보 시스템의 일반적인 특성은 '트랜잭션'을 처리해야 한다는 것이다
    -> 트랜잭션은 이련의 과정을 통해 데이터를 변경하는 것인데, command 패턴은 이런 트랜잭션의 모델링을 가능하게 한다

5. 참여자
- Command
    -> 연산 수행에 필요한 인터페이스를 선언 한다
- ComcreteCommand 
    -> Receiver 객체와 액션 간의 연결성을 정의한다
    -> 또한 처리 객체에 정의된 연산을 호출 하도록 구현 한다
- Invoker
    -> 명령어에 처리를 수행할 것을 요청 한다
- Receiver
    -> 요청에 관련된 연산 수행 방법을 알고 있습니다. 어떤 클래스도 요청 수신자로서 동작할 수 있다

6. 결과
- Command는 연산을 호출하는 객체와 연산 수행 방법을 구현하는 객체를 분리한다
- Command는 일급 클래스이다. 다른 객체와 같은 방식으로 조작되고 확장 할 수 있다
- 명령을 여러개를 복합해서 복합 명령도 만들 수 있다
- 새로운 Command 객체를 추가하기 쉽다