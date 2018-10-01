감시자 (Observer Pattern)
* 최종 정리
-

* Inflearn
- '이벤트 발생'후 '객체 외부'에서 처리할 수 있다

```java
public class Button {
    private OnClickListener onClickListener;
    
    public void onClick(){
        if(onClickListener != null){
            onClickListener.onClick(this);
        }
    }
    public interface OnclickListener{
        public void onClick(Button button);
    }
    public void setOnClickListener(OnClickListener onClickListener){
        this.onClickListner = onClickListener;
    }
}

public class ButtonClick implements OnClickListener {
    @Override
    public void onClick(Button button){
        println("CLICK!");
    }
}

public class Main {
    public static void main(String[] args){
        Button button = new Button();

        // 사용 방법1
        button.setOnClickListner(new ButtonClick());
        button.onClick();

        // 사용 방법2
        buttons.setOnClickListner(new ButtonClick(){
            @Override
            public void onClick(Button button){
                println("CLICK!");
            }
        });
    }
}
```

```java
public class Button extends Observable {
    public void onCLick(){
        setChanged();
        notifyObeservers();
    }
}
public class Main {
    public static void main(String[] args){
        Button button = new Button();
        button.addObserver(new Observer(){
            @Override
            public void update(Observable o, Object org){
                print("Click");
            }
        });
        button.onClick();
    }
}
```


* GOF
1. 의도
- 객체 사이에 일 대 다의 의존 관계를 정의해 두어, 어떤 객체의 상태가 변할 때 그 객체에 의존성을 가진 다른 객체들이
  그 변화를 통지받고 자동으로 갱신될 수 있게 만든다

2. 다른이름
- 종속자(Dependent), 게시-구독(Publish-Subscribe)

3. 동기
- 주체(Subject)는 독립된 여러 개의 감시자(Observer)가 있을 수 있다
  모든 감시자는 주체의 상태 변화가 있을 때마다 이 변화를 통보 받는다
  각 감시자는 주체의 상태와 자신의 상태를 동기화시키기 위해 주체의 상태를 알아본다 (게시 - 구독 관계)

4. 활용성
- 어떤 추상 개념이 두 가지 양상을 갖고 하나가 다른 하나에 종속적일 때. 각 양상을 별도의 객체로 캡슐화하여 이들 각각을 재사용 할때
- 한 객체에 가해진 변경으로 다른 객체를 변경해야 하고, 프로그래머들은 얼마나 많은 객체들이 변경되어야 하는지 몰라도 될 때
- 어떤 객체가 다른 객체에 자신의 변화를 통부 할 수 있는데, 그 변화에 관심 있어 하는 개체들이 누구인지에 대한 가장 없이도 통보가 될 때

5. 참여자
- Subject 
    - 감시자들을 알고 있는 주체. 임의 개수의 감시자 각체는 주체를 감시 할 수 있다
    - 주체는 감시자 객체를 붙이거나 떼는 데 필요한 인터페이스를 제공한다
- Observer
    - 주체에 생긴 변화에 관심 있는 객체를 갱신하는 데 필요한 인터페이스를 정의한다
    - 이로써 주체의 변경에 따라 변화되어야 하는 객체들의 일관성을 유지한다
- ConcreteSubject
    - ConcreteObserver객체에게 알려주어야 하는 상태를 저장한다
    - 이상태가 변경될 때 감시자에게 변경을 통보한다
- ConcreteObserver
    - ConcreteSubject 객체에 대한 참조자를 관리한다
    - 주체의 상태와 일관성을 유지해야 하는 상태를 저장한다
    - 주체의 상태와 감시자의 상태를 일관되게 유지하는 데 사용하는 갱신 인터페이스를 구현한다

6. 결과
- 주체 및 감사자 모두 독립적으로 변형하기 쉽다.
- 주체와 감시자 클래 간에는 추상적인 결합도만이 존재한다
- 브로드케스트(broadcast) 방식의 교류를 가능하게 한다
    - 감시자에게 상태 변화 사실을 알려주면 감시자가 이 통보를 처리할지 무시할지를 결정한다
- 예측하지 못한 정보를 갱신한다
    - 감시자는 다른 감시자의 존재를 모르기 때문에 주체를 변경하는 비용이 궁극적으로 어느 정도인지 모른다
