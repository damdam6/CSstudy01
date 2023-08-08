

# 옵저버 패턴(observer pattern)

주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴


>💡 주체
>
>- 객체의 상태 변화를 보고 있는 관찰자


>💡 옵저버
>
>- 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 ‘추가 변화 사항’이 생기는 객체


- 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 함
- 활용 서비스로 트위터가 있음
- 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller) 패턴에도 사용
    - 주체라고 볼 수 있는 모델에서 변경 사항이 생겨 `update()` 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러 등이 작동

## 자바에서의 옵저버 패턴

topic을 기반으로 옵저버 패턴 구현

```java
import java.util.ArrayList;
import java.util.List;

interface Subject {
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}

class Topic implements Subject {
    private List<Observer> observers;
    private String message; 

    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }

    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) observers.add(obj); 
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj); 
    }

    @Override
    public void notifyObservers() {   
        this.observers.forEach(Observer::update); 
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    } 
    
    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg; 
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this); 
        System.out.println(name + ":: got message >> " + msg); 
    } 
}

public class HelloWorld { 
    public static void main(String[] args) {
        // 주체이자 객체
				Topic topic = new Topic(); 

				// 옵저버를 선언할 때 해당 이름과 어떠한 토픽의 옵저버가 될 것인지를 정함
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c); 
   
        topic.postMessage("amumu is op champion!!"); 
    }
}
/*
Message sended to Topic: amumu is op champion!!
a:: got message >> amumu is op champion!!
b:: got message >> amumu is op champion!!
c:: got message >> amumu is op champion!!
*/
```

## 자바스크립트에서의 옵저버 패턴

프록시 객체를 통해 구현 가능


>💡 프록시 객체
>
> - 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체
> - 자바스크립트에서는 두 개의 매개변수를 가짐
>     - target : 프록시할 대상
>     - handler : 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수
> - `get()` 함수는 속성과 함수에 대한 접근을 가로챔
> - `has()` 함수는 in 연산자의 사용으로 가로챔
> - `set()` 함수는 속성에 대한 접근을 가로챔



```jsx
// set() 함수를 통해 속성에 대한 접근을 가로채서 형규라는 속성이 솔로에서 커플로 되는 것을 감시

function createReactiveObject(target, callback) { 
    const proxy = new Proxy(target, {
        set(obj, prop, value){
            if(value !== obj[prop]){
                const prev = obj[prop]
                obj[prop] = value 
                callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다`)
            }
            return true
        }
    })
    return proxy 
} 
const a = {
    "형규" : "솔로"
} 
const b = createReactiveObject(a, console.log)
b.형규 = "솔로"
b.형규 = "커플"
// 형규가 [솔로] >> [커플] 로 변경되었습니다
```