템플릿 메소드 패턴 (Templete Method Pattern)
* 최종 정리
- 알고리즘이 여러개 있고 이들을 싹다 묶어 버리는 알고리즘 하나를 만든다

* Inflearn
- '일정한 프로세스'를 가진 요구 사항을 묶어서 사용 한다
- 알고리즘의 구조를 메소드에 정의 하고 하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의 하는 패턴
- 구현하려는 알고리즘이 일정한 프로세스가 있다
- 구현하려는 알고리즘이 변경 가능성이 있다

- 알고리즘을 여러 단계로 나눈다
- 나눠진 알고리즘의 단계를 메소드로 선언 한다
- 알고리즘을 수행할 템플릿 메소들르 만든다
- 하위 클래스에서 나눠진 메소드들을 구현한다

```java
public abstract class GameConnetHelper {
    protected abstract String doSecurity(String string);
    protected abstract boolean authentication(String, id, String, password);
    protected abstract int authorization(String userName);
    protected abstract String connection(String info);

    // 템플릿 메소드
    public String requestConnection(String encodedInfo){
        String  decodedInfo = doSecurity(str); // 암호문자를 복호화 하는 과정

        String id = "aaa"
        String password = "bbb";
        if(authentication(id, password)){ // 인증과정
            throw new Error("");
        }

        String userName = "fff";
        int i = authorization(userName);
        switch(i){
            case 0:
            case 1:
        }
        return connection();
    }
}

public class DefaultGameConnectHelper extends GameConnectHelper {
    @Override
    protected String doSecurity(String string){
        return string;
    }
    @Override
    protected boolean authentication(String id, String password){
        return true;
    }
    @Override
    protected int authorization(String userName){
        return 0;
    }
    @Override
    protected String connection(String info){
        return null;
    }
}

public class Main {
    public static void main(String[] args){
        GameConnectHelper helper = new DefaultGaemConnectHelper();
        helper.requestConnection("abcdef");
    }
}
```

* GOF
1. 의도
- 객체의 연산에는 알고리즘의 뼈대만을 정의하고 각 단계에서 수행하 ㄹ구체적 차리는 서브클래스 쪽으로 미룬다
- 알고리즘의 구조 자체는 그대로 놔둔 채 알고리즘 각 단계 처리를 서브클래스에서 재정의할 수 있게 한다

2. 동기
- 추상 연산을 통해서 알고리즘의 일부 단계를 정의함으로써, 템플릿 메서드는 각 단계의 순서는 고정하되 
  필요에 따라 이들 단계의 처리를 다양화시킬 수 있도록 한다

3. 활용성
- 어떤 한 알고리즘을 이루는 부분 중 변하지 앟는 부분을 한번 정의해 놓고 다양해 질수 있는 부분은 서브클래스에서 정의 할수 있도록한다
- 서브클래스 사이의 공통적인 행동을 추출하여 하나의 공통 클래스에 몰아둠으로써 코드 중복을 피하고 싶을 때
- 서브클래스의 확장을 제어할 수 있다

4. 참여자
- AbstractClass
    - 서브 클래스들이 재정의를 통해 구현해야 하는 알고리즘 처리 단계 내의 기본 연산을 정의한다
    - 알고리즘의 뼈대를 정의하는 템플릿 메서드를 구현 한다
- ConcreteClass
    - 서브클래스마다 달라진 알고리즘 처리 단계를 수행하기 위한 기본 연산을 구현한다

5. 결과
- 부모 클래스는 서브클래스에 정의된 연산을 호출 할 수 있지만 반대 방향의 호출은 안 된다
