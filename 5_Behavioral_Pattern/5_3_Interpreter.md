해석자 패턴 (Interpreter Pattern)
* 최종 정리
- 해석자 패턴은 사용자 표현하기 쉬운 표현을 사용하게 하고 이를 해석하는 객체를 통해 약속된 알고리즘을 수행할 수 있게 해주는 패턴
- 사용자가 원하는 다양한 명령르 쉽게 표현할 수 있게 구문 약속을 해야 한다
- 즉, 사용자가 2 Add 3 이렇게 쓰면 이걸 해석 해서 2+3을 해줘야 한다

- 오리가 뒤뚱뒤뚱 걷다
- 오리가 팔랑팔랑 걷다
- 오리가 살랑살랑 걷다
-> [누가] [어떻게] [행동] 이렇게 나눠서 설계를 하면 엔진 입장에서는 앞으로 모든 문장을 신경쓰지 않고 모듈화 해서 치라 할 수 있다

```java
interface Expression {
     boolean interpret();
}
public class TerminalExpression implements Expression {
    private String data;
    public TerminalExpression(String data){
        this.data = data;
    }
    @Override
    public boolean interpret(String context){
        if(context.contains(data)){
            return true;
        }
        return false;
    }
}
public class OrExpression implements Expresssion {
    private Expression expr1 = null;
    private Expression expr2 = null;

    public OrExpression(Expression expr1, Expression expr2){
        this.expr1 = expr1;
        this.expr2 = expr2;
    }
    @Override
    public boolean interpret(String context){
        return expr1.interpret(context) || expr2.interpret(context);
    }
}
public class AndExpression implements Expression {
    private Expression expr1 = null;
    private Expression expr2 = null;

    public AndExpression(Expression expr1, Expression expr2){
        this.expr1 = expr1;
        this.expr2 = expr2;
    }
    @Override
    public boolean interpret(String context){
        return expr1.interpret(context) || expr2.interpret(context);
    }
}
public class InterpreterPatternDemo{
    public static Expression getMaleExpression(){
        Expression robert = new TerminalExpression("Robert");
        Expression john = new TerminalExpression("John");
        return new OrExpression(robert, john);
    }
    public static Expression getMarriedWomanExpression(){
        Expression julie = new TerminalExpression("Julie");
        Expression married = new TerminalExpression("Married");
        return new AndExpression(julie, married);
    }
    public static void main(String[] args){
        Expression isMale = getMaleExpression();
        Expression isMarriedWoman = getMarriedWomanExpression();

        print("John is male?" + isMale.interpret("John"));
        print("Julie is a married woman?" + isMarriedWoman.interpret("Married Julie"));
    }
}
```

* GOF
1. 의도
- 어떤 언어에 대해, 그언어의 문법에 대한 표현을 정의하면서 그것을 사용하여 해당 언어로 기술된 문장을 해석하는 해석자를 함께 정의한다

2. 동기
- 특정한 종류의 문제가 자주 발생할 때는, 어떤 간결한 언어를 써서 그문제를 문장으로 표현하는 것이 나을 수 있다
  그리고 나서 그 문장을 해석하는 해석자를 만들어 문장을 해석하게 하여 문제를 해결 하는 것

3. 활용성
- 해석이 필요한 언어가 존재하거나 추상 구문 트리로서 그 언어의 문장을 표현하고자 할 경우
- 정의할 언어의 문법이 간단할 경우
    -> 복잡하면 파서 생성기와 같은 도구를 이용하느 ㄴ것이 더 나은 방법이다
- 효율성은 별로 고려할 상항이 아닐 경우

4. 참여자
- AbstractExpression   
    -> 추상 구문 트리에 속한 모든 노드에 해당하는 클래스들이 공통적으로 가져야 할 Interpret()연산을 추상 연산으로 정의한다
- TerminalExpression
    -> 문법에 정의한 터미널 기화와 관련된 해석 방법을 구현한다
    -> 문장을 구성하는 모든 터미널 기호에 대해서 해당 클래스를 만들 어야 한다
- NonterminalExpression 
    -> 문법의 오른편에 나타나는 모든 기호에 대해서 클래스를 정의해야 한다
- Context
    -> 번역기에 대한 포괄적인 정보를 포함한다
- Client
    -> 언어로 정의한 특정 문장을 나타내는 추상 구문 트리이다
    -> 이 추상 구문 트리는 NonterminalExpression과 TerminalExpression 클래스의 인스턴스로 구형 된다
    -> Interpret()연산을 호출 한다

5. 결과
- 문법의 변경과 확장이 쉽다
- 문법의 구현이 용이하다
- 복잡한 문법은 관리하기가 어렵다
- 표현식을 해석하는 새로운 방법을 추가할 수 있다