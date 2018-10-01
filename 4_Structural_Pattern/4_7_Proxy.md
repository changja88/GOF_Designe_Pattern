프록시 패턴(Proxy pattern)
* 최종 정리
- 프록시 = 대리인 
- 즉, 진짜로 클래스가 생성되서 해야할 일이 아니라면 프로시클래스가 대신 일을 처리 해주는 방식


* Inflearn
- 프록시 패턴을 통해 작업을 '나눠서' 구현 할 수 있다
- 프록시가 처리 할 수 있는건 프록시가 하고, 할 수 없는건 실제 작업을 처리 하는 애가 한다

```java
public interface Subject {
    void action1(); // 리소스가 적게 드는 일
    void action2(); // 리소스가 많이 드는 일 (네트워크, 메모리 많이 소모)
}
public class RealSubject implements Subject {
    @Override 
    public void action1(){
        print("간단한 업무")
    }
    @Override
    public void action2(){
        print("복잡한 업무")
    }
}
public class Proxy implements Subject {
    private Subject real;

    public Proxy(RealSubject real){
        this.real = real;
    }

    @Override
    public void action1(){
        print("간단한 업무")
    }
    @Override
    public void action2(){
        real.action2();
    }
}

public class Main {
    public static void Main(String[] args){
        Subject real = new RealSubject();

        Subject proxy1 = new Proxy(real);
        Subject proxy2 = new Proxy(real);

        proxy1.action1();
        proxy2.action1();    

        proxy1.action2();
        proxy2.action2();        
    }
}
```

* GOF
1. 의도 : 다른 객체에 대한 접근을 제어하기 위한 대리자 또는 자리채움자 역할을 하는 객체를 둔다

2. 다른 이름 : 대리자(Surrogate)

3. 동기
- 어떤 객체에 대한 접근을 제어하는 한 가지 이유는 실제로 그 객체를 사용할 수 있을 때까지 객체 생성과 초기화에 들어가는 
  비용 및 시간을 물지 않겠다는 것
- 여러 제약 사항들로 생성이나 관리가 어려운 객체라면 꼭 필요할 때만 이 객체르 생성하도록 하는 방법

4. 활용성
- 원격지 프록시(remote proxy)
    -> 서로 다른 주소 공간에 존재하는 객체를 가리키는 대표 객체로, 로컬 환경에 위치한다
- 가상 프록시(virtual proxy)
    -> 요청이 있을 때만 필요한 고비용 객체를 생성한다
- 보호용 프록시(protection proxy)
    -> 원래 객체에 대한 실제 접근을 제어 한다
    -> 객체별로 접근 제어 권한이 다를 때 유용하게 사용할 수 있다
- 스마트 참조자(smart reference)
    -> 원시 포인터의 대체용 객체로, 실제 객체에 접근이 일너라 때 추가적이 ㄴ행동을 수행한다
    -> 실제 객체에 대한 참조 횟수를 저장하다가 더는 참조가 없을 때 해당 객체를 자동으로 없에준다
    -> 맨 처음 참조되는 시점에 영속적 저장소의 객체를 메모리로 옮긴다
    -> 실제 객체에 접근 하기 전에, 다른 객체가 그것을 변경하지 못하도록 실제 객체에 대한 잠금(lock)을 건다

5. 참여자
- Proxy
    -> 실제로 참조할 대상에 대한 잠조자를 관리한다
    -> RealSubject 와 subject 인터페이스가 동일하면 프록시는 subject에 대한 참조자를 갖는다
    -> subject와 동일한 인터페이스를 제공하여 실제 대상을 대체 할 수 있어야 한다
    -> 실제 대상에 대한 접근을 제어하고 실제 대상의 생성과 삭제를 책임진다
    -> 프록시 종류에 따라서 다음을 수행한다
        -> 원격지 : 요청 메시지와 인자를 인코딩하여 이를 다른 주소 공간에 있는 실제 대상에게 전달한다
        -> 가상 : 실제 대상에 대한 추가적 정보를 보유하여 실제 접근을 지연 할 수 있도록 한다
        -> 보호용 : 요청한 대상이 실제 요청할 수 있는 권한이 있는지 확인한다
- Subject
    -> RealSubject와 Proxy에 공통적인 인터페이스를 정의하여, Realsubject가 요청되는 곳에 proxy를 사용할 수 있게 한다
- RealSubject
    -> 프로시가 대표하는 실제 객체 이다

6. 결과
- 프록시 패턴은 어떤 객체에 접근 할 때 추가적인 간접화 통로를 제공한다
    -> 원격지 프록시는 개체가 다른 주소 공간에 존재한다는 사실을 숨길 수 있다
    -> 가상 프록시는 요규에 따라 객체를 생성하는 등 처리에 최적화할 수 있다
    -> 보호용 프록시 및 스마트 참조자는 객체가 접근할 때마다 추가 관리를 책임진다