책임 연쇄 (Chain Of Responsiblilty)
* 최종 정리
- 요청을 하나 날리면 그 요청을 해결 할 수 있는 객체가 나올 때가지 요청을 패스 하는 방식
- 또는, 요청을 하나 날리면 그 요청을 객체 1도 처리 하고 객체 2도 처리 ... 하는 방식

* Inflearn
- '다양한 처리' 방식을 '유연'하게 처리 할수 있다
- ex>
```java
public abstract class Calculator {
    private Calculator nextCalculator;
    public void setNextCalculator(Calculator nextCalculator){
        this.nextCalculator = nextCalculator;
    }
    public boolean process(Request request){ // 다음에 책임질 놈을 불러 오는 코드
        if(operator(request)){            
            return true;
        }else {
            if(nextCalculator != null){
                return nextCalculator.process(request);
            }
        }
        return false
    }
    abstract protected boolean operator(Request request); // 각자 구현 해야 할 코드
}
public class Request {
    private int a,b;
    private String operator;
}
public class PlusCalculator extends Calculator {
    @Override
    protected boolean operator(Request request){
        if(request.getOperator().equals("+")){
            int a = request.getA();
            int b= request.getB();
            int result = a + b 
            print(result);
            return true;
        }
        return false;
    }
}
public class SubCalculator extends Calculator {
    @Override
    protected boolean operator(Request request){
        if(request.getOperator().equals("-")){
            int a = request.getA();
            int b= request.getB();
            int result = a - b 
            print(result);
            return true;
        }
        return false;
    }
}
public class Application {
    public static void main(String[] args){
        Calculator plus = new PlusCalculator();
        Calculator sub = new SubCalculator();

        plus.setNextCalculator(sub);
        Request request1 = new Request(1,2,"+");
        Request request2 = new Reuqest(10,2,"0");
        plus.process(request1);
        plus.process(request2);
    }
}
```
```java
public class Attack {
    private int amount;
    public Attack(int amount){
        this.amount = amount;
    }
}
public interface Defense {
    public void defense(Attack attack);
}
public class Amor implements Defense{
    private Defense nextDefense;
    private int def;

    @Override
    public void defense(Attack attack){
        process(attack);
        if(nextDefense != null){
            nextDefense.defense(attack);
        }
    }
    private process(){
        int amount = attack.getAmout();
        amount = attack - def;
        attack.setAmount(amount);
    }
}
public class Application {
    public static void main(String[] args){
        Attack attack = new Attack(100);

        Armor armor1 = new Armor(10);
        Armor armor2 = new Armor(15);

        armor1.setNextDefense(armor2);
        armor1.defense(attack);
    }
}
```
* GOF
1. 의도
- 메세지를 보내는 객체와 이를 받아 처리하는 개체들 간의 결합도를 없애기 위한 패턴
- 하나의 요청에 대한 처리가 반드시 한 객체에서만 되지 않고 여러 객체에게 그 처리 기회를 주려는 목적

2. 동기
- 메시지 송신 측과 수진 측을 분리 하기 위함
- 요청을 처리하는 기회를 다른 객체에 분산하기 위함

3. 활용성
- 하나 이상의 객체가 요청을 처리해야 하고, 그 요청 처리자 중 어떤 것이 선행자 인지 모를때, 처리자가 자동으로 확정 되어야 한다
- 메세지를 받을 객체를 명시하지 않은 채 여러 객체 중 하나에게 처리를 요청 하고 싶을 때
- 요청 처리할 수 있는 객체 집합이 동적으로 정의되어야 할 때

4. 참여자
- Handler 
    -> 요청을 처리하는 인터페이스를 정의하고, 후속 처리자와 연결을 구현한다.
    -> 즉, 연결 고리에 연결된 다음 객체에게 다시 메시지를 보낸다
- ConcreteHandler
    -> 채임져야 할 행동이 있다면 스스로 요청을 처리하여 후속 처리자에 접근 할 수 있다
    -> 즉, 자신이 처리할 행동이 있으면 처리하고, 그렇지 않으면 후속 처리자에게 다시 처리를 요청한다

5. 결과
- 객체 간의 행동적 결합도가 적어진다
    - 다른 객체가 어떻게 요청을 처리하는지 몰라도 된다
    - 메시지를 보내는 측이나 받는 측 모두 서로를 모르고, 또 연결된 객체들 조차도 그 연결 구조가 어떻게 되는지 모른다
    - 결과적으로, 객체들 간의 상호작용 과정을 단순화 시킨다
- 객체에게 책임을 할당하는 데 유연성을 높일 수 있다
    - 객체의 책임을 여러 객체에게 분산시킬 수 있으므로 런타임에 객체 연결 고리를 변경하거나 추가 하여 책임을 변경하고나 확장 할 수 있다
- 메시지 수신이 보장되지는 않는다
    - 어떤 객체가 이 처리에 대한 수신을 담당한 다는 것을 명시하지 않으므로 요청이 처리된다는 보장은 없다
    