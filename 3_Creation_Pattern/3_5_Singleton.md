단일체(Singleton)
* 최종 정리

* Inflearn
- 객체 : 속성과 기능을 갖춘 것 (자동차)
- 클래스 : 송성과 기능을 정의 한 것 (자동차 설계도)
- 인스턴스 : 속성과 기능을 가진 것 중 실제 하는 것 (공장에서 만들어진 자동차 하나한)

- 학습 목표
-> 싱글톤 패턴을 통해서 '하나의 인스턴스'만 '생성하도록 구현' 할 수 있다

- 요구 사항
-> 개발 중인 시스템에서 스피커에 접근 할 수 있는 클래스를 만들어 주세요

- 예제
```java
public class SystemSpeaker {
    static private SystemSpeaker instance;
    private int volume;

    private SystemSpeaker(){
        volume = 5
    }
    public static SystemSpeak getInstance(){
        // 이렇게 해야 객체가 딱 한번만 생성이 된다
        if(instance == null){
            instance = new SystemSpeaker();
        }
        return instance;
    }
}

public class Main {
    public static void main(String[] args){
        SystemSpeaker speaker = new SystemSpeaker() // -> 이렇게 할 수 없음 (생성자가 private이기 때문)
        SystemSpeaker speaker1 = SystemSpeaker.getInstance(); // -> 이런식으로 단일체를 호출해야한다
        SystemSpeaker speaker1 = SystemSpeaker.getInstance();
        // speaker1, speaker2 는 완전히 동일한 spekaer이다 -> 하나의 볼륨을 바꾸면 둘다 바뀐다
    }
}
```

* GOF
1. 의도 : 오직 한개의 클래스 인스턴스만 갖도록 보장하고, 이데 대한 전역적인 접근점을 제공한다

2. 동기 
- 어떤 클래스는 정확히 하나의 인스턴스만을 갖도록 하는 것이 좋습니다
- 시스템에 많은 프린터가 있다 하더라도, 프린터 스풀은 오직 하나여야 한다
- 파일 시스템도, 윈도우 관리자도 오직 하나여야 한다
- 전역 변수를 이용해서 이 객체에 접근하도록 하면 여러개의 니스턴스를 만들 필요가 없지만, 단일체로 만드는 것이 관리하기에 좋다
- 클래스 자신이 그 인스턴스에 대한 접근 방법을 제공할 수 있고 이를 가리켜 단일체 패턴이라고 한다

3. 활용성
- 클래스의 인스턴스가 오직 하나여야 함을 보장하고, 잘 정의된 접근점(access point)로 모든 사용자가 접근할 수 있도록 해야 할 때 
- 유일한 인스턴스가 서브클래싱으로 확장되어야 하며, 사용자는 코드의 수정 없이 확장된 서브클래스의 인스턴스를 사용할 수 있어야 할 때

5. 참여자
- Singleton 
-> Instance() 연산을 정의하여, 유일한 인스턴스로 접근할 수 있도록 한다 Instance()연산은 클래스 연산이다
-> 유일한 인스턴스를 생성하는 책임을 맡는다

6. 협력방법
- 사용자는 Singleton 클래스에 정의된 Instance()연산을 통해서 유일하게 생성되는 단일체 인스턴스에 접근 할 수 있다

7. 결과
1. 유일하게 존재하는 인스턴스로의 접근을 통제한다
-> 싱클톤 클래스 자체가 인스턴스를 캡슐화하기 때문에, 이 클래스에서 사용자가 언제, 어떻게 이 인스턴스에 접근 할 수 있는제 제어 가능하다
2. 이름 공간(name space)를 좁힌다
-> 단일체 패턴은 전역 변수보다 좋다. 전역 변수를 사용해서 이름 공간을 망치는일을 없애주기 때문이다. 
-> 즉, 전연 변수를 정의하여 발생하는 디버깅의 어려움 등의 문제를 해결해준다
3. 연산 및 표현의 정제를 허용한다
-> 싱글톤 클래스는 상속될 수 있기 때문에, 이 상속된 서브클래스를 통해서 새로운 인스턴스를 만들 수 있다
-> 또한 이패턴을 사용하면, 런타임에 필요한 클래스의 인스턴스를 써서 응용프로그램을 구성할 수도 있다
4. 인스턴스의 개수를 변경하기가 자유롭다
-> 마음이 바뀌어서 싱글톤 클래스의 인스턴스가 하나 이상 존재할 수 있도록 변경해야 할 때도 있는데, 쉽게 할 수 있다

8. 구현
- 인스턴스가 유일해야 함을 보장한다
-> 단일체 패턴은 클래스의 인스턴스가 오로지 하나임을 만족해야 한다
-> 일반적인 방법은 인스턴스를 생성하는 연산을 클래스 연산으로 만드는 것이다
-> 이 연산은 유일한 인스턴스를 관리할 변수에 접근해서 이 변수에 유일한 인스턴스로 초기화하고, 이 변수를 되돌려 줌으로써 사용자가 유일한
   인스턴스를 사용할 수 있도록 한다
-> 이 방법은 단일체 클래스의 인스턴스가 처음 사용되기 바로 직전에 그 인스턴스를 생성하고 초기화하도록 보장한다
-> 사용자는 반드시 Instance() 함수를 통해서만 인스턴스에 접근해야 한다

- 싱글톤 클래스를 서브클래싱 한다
-> 서브클래스를 만드는 것이 중요한 것이 아니라, 새로운 서브클래스의 유일한 인스턴스를 만들어 사용자가 이를 사용 할 수 있도록 하는 것이다
-> 핵심은 싱글톤의 인스턴스를 참조하는 변수가 이들 서브클래스의 인스턴스로 초기화되어야 한다는 것이다
-> 가장 쉬운 방법은 싱글톤 클래스의 Instance() 연산을 사용할 때 어떤 단일체를 사용할지 결정하는 방식으로, 슈퍼클래스의
   단일체를 사용할지, 서브클래스의 단일체를 사용할지를 결정 하는 방법이 이다
-> 싱글톤의 서브클래스를 선택하는 또 다른 방법은 Instance( )연산의 구현을 슈퍼클래스가 아닌 서브클래스에서 하는 것이다
   이는 가능한 싱글톤 클래스들에 대한 정보를 직접 코드에 작성하는 것이므로 좋지 않다
-> 유연한 방법은 '단일체에 대한 레지스트리'르 ㄹㅈ사용하는 것이다
   Instance()연산에 간으한 단일체 클래스 집합을 정의하는 대신에 단일체 클래스는 이 단일체 인스턴스를 레지스트리에 이름을 갖는
   인스턴스로 등록한다. 레지스트리는 문자열로 정의된 이름에 해당 단일체 인스턴스로 대응시켜둔다 
   Instance()연산에서 단일체가 필요할 때 레지스트리를 뒤져서 이름으로 해당 단일체를 찾아다랄고 의뢰하면 레지스트리는 해당 
   단일체를 찾아서 돌려주는 것이다.
   이방식을 사용하면 Instance()연산이 모든 단일체 클래스와 인스턴스를 알 필요가 없다(유연한다)
ex>
```java
class Singleton {
    public:
        static void Register(const cahr* name, Singleton*); //이름과 singleton 인스턴스를 등록한다
        static Singleton* Instance();
    protected:
        static Singleton* Lookup(const char* name);
    private:
        static Singleton* _instance;
        static List<NameSingletonPair>* _registry;
};

Singleton* Singleton::Instance(){
    if(_instance ==0){
        const char* singletonName = getenv("SINGLETON"); // 사용자 또는 환경 변수는 시작 시에 이것을 지원한다
        _instance = Lookup(singletonname); // 해당하는 singleton이 없다면 0을 반환한다
    }
    return _instance;
}

MySingleton::MySingleton(){
    // 생성자 안에서 슈퍼클래스의 Register()연산을 이용하여 자신을 등록하고 있다
    Singleton::Register("MySingleton", this);
}
```
9. 예제코드
```java
class MazeFactory {
    public :
        static MazeFactory* Instance();
        // 다른 인터페이스는 여기에 정의한다
    protected :
        MazeFactory();
    private :
        static Mazefactory* _instance;
};

MazeFactory* MazeFactory::_instance =0;

MazeFactory* MazeFactory::Instance() {
    if(_instance == 0){
        _instance = new MazeFactory;
    }
    return _instance;
}

MazeFactory* MazeFactory::Instance() {
    if(_instance == 0){
        const char* mazeStyle = getenv("MAZESTYLE");
        if(strcmp(mazeStyle, "bombed") == 0){
            _instance = new BombedMazeFactory;
        }else if(strcmp(mazeStyle, "enchanted") == 0){
            _instance = new EnchangtedMazeFactory;
            // 다른 가능한 서브클래스들
        }else {
            // 기본 인스턴스 생성
            _instance = new MazeFactory;
        }
    }
    return _instance;
}
```


