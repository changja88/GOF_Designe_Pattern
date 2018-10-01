복합체(Composite pattern)
* 최종 정리
- 컨터이너 와 내용물을 동일하게 다루기 위한 패턴
-               과자(Component)
-   새우깡   쿠쿠다스       과자세트A(Leaf)
-                  새우깡 쿠쿠다스 과자세트B(Composite)
- 이런식으로 구조가 생기기 때문에 트리 구조가 만들어질 수 있다 (과자세트 부분)
- Component : 과자
- Leaf : 새우깡, 쿠쿠다스
- Composite : 과자세트

* Inflearn
```java
abstract public class Component {
    private String name;
    public Component(String name ){
        this.name = name;
    }
}

public class File extends Component {
    private Object data;
}
public class Folder extends Component {
    List<Component> children = new ArrayList<>();
    public boolean addComponent(Component component){
        return childern.add(component); //Composite는 leaf들의 리스트를 들고 있어야 한다
    }
}
public class Main {
    public static void main(String[] args){
        Folder
        root = new Folder("root"),
        home = new Folder("home"),
        garma = new Folder("garma"),
        music = new Folder("music"),
        picture = new Folder("picture")
        doc = new Folder("doc")

        File
        track1 = new File("trakc1"),
        track2 = new File("trakc2"),
        pic1 = new File("pic1"),
        doc1 = new File("doc1"),
        java = new File("java")

        root.addComponent(home);
            home.addComponent(garma);
                garam.addComponent(picture);
                    picture.addComponent(pic1);
                    picture.addComponent(pic2);
                garam.addComponent(picture);
                garam.addComponent(doc);
        root.addComponent(usr);
        // 이런식으로 트리구조가 만들어 진다        
    }
}
```
* GOF
1. 의도
- 부분과 전체의 계층을 표현하기 위해 객체들을 모아 트리 구조로 구성한다
- 사용자로 하여금 개별 객체와 복합 객체를 모두 동일하게 다룰 수 있도록 하는 패턴이다

2. 동기

3. 활용성
- 부분 - 전체의 객체 계통을 표현하고 싶을 때
- 사용자가 객체의 합성으로 생긴 복합 객체와 객개의 객체 사이의 차이를 알지 않고도 하고 싶은 일을 하게 하고 싶을때

4. 참여자
- Component 
-> 집합 관계에 정의될 모든 객체에 대한 인터페이스를 정의한다
-> 모든 클래스에 해당하는 인터페이스에 대해서는 공통의 행동을 구현한다
- Leaf
-> 자식 객체, 객체 합성에 가장 기본이 되는 객체의 행동을 정의한다
- Composite
-> 자식이 있는 구성요소에 대한 행동을 정의한다 
-> 자식 관련 연산을 구현 한다

5. 결과
- 기본 객체와 복합 객체로 구성된 하나의 일관된 클래스 계종을 정의한다
- 런타임에 기본 객체와 복합 객체를 구분하지 않고 일관되게 프로그래밍을 할 수 있다
- 새로운 종류의 구성요소를 쉽게 추가할 수 있다
    -> 새롭게 정의된 Composite나 Leaf의 서브클래스들은 기본에 존재하는 구조와 독립적으로 동작이 가능하게 된다
    -> 그러므로 새로운 요소가 추가 됬다고해서 사용자의 코드는 변경될 필요가 업삳
- 설계가 지나치게 범용성을 많이 가지게 된다
