적응자(Adapter)
* 최종 정리
- 원래 알고르즘이나 함수를 내가 원하는 방식(나의 요구사항에 맞게)으로 살짝 고쳐서 사용 하는 방식
- 서로 비슷한 기능을 하지만 사용 방법이 다른 경우 같은 인터페이스를 갖게 변환시키는 패턴

* Inflearn
- 연관성 없는 두 객체 묶어 사용하기
- 요구 사항
-> 두수에 대한 다음 연산을 수행하는 객체를 만들어라
-> 수의 두배수를 반환 (Float)
-> 수의 반의 수를 반환 (Float)
-> 구현 객체의 이름은 'Adapter'
-> Math 클래스에서 두 배와 절반을 구하는 함수는 '이미 구현'되어 있다
- ex>
```java
public class Math {
    public static double twoTime(double num) { return num*2; }
    public static double half(double num) { return num/2; }
}
public interface Adapter {
    // 원하는 기능
    public Float twiceOf(Float f);
    public Float halfOf(Float f);
}
public class AdapterImpl implements Adapter {
    Math math;
    @Override
    public Float twiceOf(Float f){
        Log.d("test","LOG"); // 원래 있는 twiceOf함수를 내가 원하는 방식을 추가하여 사용 한다 (adpating)
        return (float)Math.twiceOf(f.doubleValue())
    }
    @Override
    public Float halfOf(Float f){
        return (float)Math.half(f.halfValue())
    }
}
public class Main {
    public static void main(String[] args) {
        AdpaterImple adapter = new AdapterImpl();
        adapter.twiceOf(100)// ->200
    }
}
```
- ex> Shape
```java
public interface TextViewInterface {
    public int methodA();
}

public class TextView implements TextViewInterface {
    public int methodA();
}
// 여기까지는 기존에 사용하던 코드

// 여기부터는 새로 개발하는 코드
public interface Shape {
    public int BoundingBox();
    public int CreateManipulator();
}

public class TextShape implements Shape {
    private TextView textView;
    
    public int BoundingBox() {
        self.textView....
    }
    public int CreateManipulator() {
    }
}
```

* GOF
1. 의도 
- 클래스의 인터페이스를 사용자가 기대하는 인터페이스 형태로 적응(변환)시킨다
    -> 서로 일치하지 않는 인터페이스를 갖는 클래스들을 함께 동작시킨다 

2. 다른 이름
- 래퍼(Wrapper)

3. 동기
- 가끔은 재사용을 목표로 개발한 툴킷도 실제 재사용성을 발휘하지 못할 때가 발생한다
- 응용프로그램이 요청하는 인터페이스와 툴킷에 정의된 인터페이스가 일치하지 않을 때가 있기 때문이다
- 이미 존재하기는 하지만 현재 이를 사용하고자 하는 클래스와는 아무런 연관 없이 개발될 클래스이거나, 서로 일치하지 않는 인터페이스를 갖는 클래스들을 잘 통합하여 하나의 응용프로그램을 개발해야 할때 -> 이미 만든 클래스들을 잘 통합하여 새로운걸 만들고 싶다
- 이미 개발된 클래스의 인터페이스를 수정할 수 없다면, 양쪽 모두 맞도록 우리가 새로 개발할 클래스를 조정해야 한다
    -> 합치려고 하는 두 클래스를 모두 상속 받는다 (적응자 클래스 버젼)
    -> TextView 의 인스턴스를 TextShape에 포함시키고, TextViewInterface 인터페이스를 사용하여 TextShape을 구현한다 (적응자 객체 버젼)
        -> 이때 TextShape을 적응자라고 한다

4. 활용성
- 기존 클래스를 사용하고 싶은데 인터페이스가 맞지 않을 때
- 아직 예측하지 못한 클래스나 실제 관련되지 않은 클래스들이 기존 클래스를 재사용하고자 하지만, 이미 정의된 재사용 가능한 클래스가
  지금 요청하는 인터페이스를 꼭 정의하고 있지 않을 때
  즉, 이미 만든 것을 재사용하고자 하나 이 재사용 가능한 라이브러리르 수정할 수 없을 때
- (객체 적응자만 해당됨) 이미 존재하는 여러 개의 서브클래스를 사용해야 하는데, 이 서브클래스들의 상속을 통해서 이들의 인터페이스를
  다시 개조한다는 것이 현실성이 없을 때, 객체 적응자를 써서 부모 클래스의 인터페이스를 변경하는 것이 더 바람직 하다

5. 

```java
class Shape {
    public :
        Shape();
        virtual void BoundingBox (
            Point& bottomLeft, Point& topRight
        ) const;
        virtual Manipulator* CreateManipulator() const;
};

class TextView {
    public : 
        TextView();
        void GetOrigin(Coord& x, Coord& y) const;
        void GetExtent(Coord& width, Coord& height) const;
        virtual bool IsEmpty() const;
}
// TextView 클래스가 Shape가 원하는 형태의 연산 이름으로 서비스를 제공하고 있지 않을 때라도 TextView 클래스를 사용
// 하고자 한다면 적응자를 만들어야 한다

class TextShape : public Shape { // 이게 Apater 클래스가 된다
    public 
        TextShape(TextView*);

        virtual void BoundingBox(
            Point& bottomLeft, Point& topRight
        ) const;
        virtual bool IsEmpty() const;
        virtual Manipulator* CreateManipulator() const;

        TextView* _text; // TextView에 대한 참조자
}
TextShape:: TextShape(TextView* t){
    _text = t;
}
void TextShape::BoundingBox (
    Point& bottomLeft, Point& topRight
)const {
    Coord bottom, left, widht, height;
    _text->GetOrigin(bottom, left);
    _text->GetExten(width, height);

    bottomLeft = Point(bottom, left);
    topRight = Point(bottom + height, left + width);
}

bool TextShape::IsEmpty() const{
    return _text->IsEmpty();
}
// 중요 포인트 나는 TextView를 주로 쓸건데 Shape에 있는 몇가지 기능이 필요하다
// TextShape라는 Adapter를 만들어서 Shape을 상속 받고 Shape중에서 쓰고 싶은 기능을 TexwView 멤버를 이용하여 구현한다 
```
