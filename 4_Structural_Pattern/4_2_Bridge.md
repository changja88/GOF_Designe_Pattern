가교(Bridge)
* 최종 정리
- 구현이 있는 인터페이스를 이용 하여 기능 계층과 구현 계층을 분리 시킨다
- 하나의 인터페이스를 만들고 그것을 다양하게 구현하여 여러가지 방법으로 사용 한다

* Inflearn
- 기능 계층과 구현 계층의 분리
- ex>
```java
// 패턴 미적용
public class MorseCode {
    public void dot(){
        // print(".");      // 처음에 구현 했던 코드
        System.call.Bip();  // 새로 바꾸고 싶은 코드
    }
    public void dash(){
        // print("-");
        System.call.LongBip();
    }
    public voidspace(){
        // print(" ");
        System.call.longTip();
    }
}
public class PrintMorseCode extends MoreseCode {
    public PrintMorseCode g(){
        dash();dash();dot();space();
        return this;
    }
    public PrintMorseCode a(){
        dot();dash();space();
        return this;
    }
    public PrintMorseCode r(){
        dot();dash();dot();space();
        return this;
    }
    public PrintMorseCode m(){
        dash();dash();space();
        return this;
    }
}
public class Main {
    public static void main(String[] args){
        PrintMorseCode code = new PrintMorseCode();
        code.g();code.a();code.r();code.a();code.m();
        code.g().a().r().a().m(); //체이닝
    }
}

// 패턴 적용
public interface MorseCodeFunction { // 가교
    public void dot();
    public void dash();
    public void space();
}

public class DefaultMorseCodeFunction implements MorseCodeFunction{
    @Override
    public void dot(){
        print("."); 
    }
    @Override
    public void dash(){
        print("-");
    }
    @Override
    public voidspace(){
        print(" ");
    }
}
// 계속 이런식으로 추가 해서 만들 수 있음
public class SoundMorseCodeFunction implements MorseCodeFunction{
    @Override
    public void dot(){
        System.call.longTip();
    }
    @Override
    public void dash(){
        System.call.longTip();
    }
    @Override
    public voidspace(){
        System.call.longTip();
    }
}

public class MorseCode {
    private MorseCodeFunction function;

    public MorseCode(MorseCodeFunction function){
        this.function = function;
    }
    public void dot(){
        function.dot(); // delegate 패턴
    }
    public void dash(){
        function.dash();
    }
    public void space(){
        function.space();
    }
}


public class Main {
    public static void main(String[] args){
        PrintMorseCode code = new PrintMorsCode(new DefaultMorseCodeFunction());
        code.g().a().r().a().m(); //체이닝

        PrintMorseCode code = new PrintMorsCode(new SoundMorseCodeFunction());
        code.g().a().r().a().m(); //체이닝
    }
}
```

* GOF
1. 의도 : 구현에 추상에 분리하여, 이들이 독립적으로 다양성을 가질 수 있도록 하기 위함

2. 다른이름 : 핸들/구현부 (Hadle/Body)

3. 동기
- 하나의 추상적 개념에 여러가지 구현으로 구체화될 수 있을 때, 대부분은 상속을 통해서 이 문제를 해결한다
- 하지만 상속을 사용하면 구현과 추상적 개념을 영구적으로 종속 시키기 때문 수정, 확장 하기가 쉽지 않다 -> 가교 패턴이 필요 함

4. 활용성
- 추상적 개념과 이에 대한 구현 사이의 지속적인 종속 관계를 피하고 싶을 때
- 런타임에 구현 방법을 선택하거나 구현 내용을 변경하고 싶을 때
- 추상적 개념과 구현 모두가 독립적으로 서브클래싱을 통해 확장되어야 할 때
- 추상적 개념에 대한 구현 내용을 변경하는 것이 다른 관련 프로그램에 아무런 영향을 주지 않아야 할 때
- 여러 객체들에 걸쳐 구현을 공유하고자 하며, 또 이런 사실을 사용자 쪽에 공개하고 싶지 않을 때

5. 참여자
- Abstraction : 추상적 개념에 대한 인터페이스를 제공하고 객체 구현자에 대한 참조자를 관리한다
- RefinedAbstraction : 추상적 개념에 정의된 인터페이스를 확장 한다
- Implementor 
    -> 구현 클래스에 대한 인터페이스를 제공한다
    -> 실질적인 구현을 제공한 서브클래스들에 공통적인 연산의 시그니처만을 정의한다
- ConcreteImplementor : 인터페이스를 구현하는 것으로 실제적인 구현 내용을 담는다

6. 결과
- 인터페이스와 구현 분리
    -> 구현이 인터펭스에 얽매이지 않게 된다
    -> 추상적 개념에 대한 어떤 방식의 구현을 택할지가 런타임에 결정될 수 있다
    -> 이는 런타임에 어떤 객체가 자신의 구현을 수시로 변경할 수 있음을 의미한다
- 확장성 제고
    -> Abstraction과 Implementor를 독립적으로 확장 할 수 있다
- 구현 세부 사항을 사용자에게서 숨기기

7. 구현
- Implementor를 하나만 둔다
    -> 