# CS 1장 디자인 패턴과 프로그래밍 패러다임

# 1장 디자인 패턴과 프로그래밍 패러다임

- 라이브러리
    - 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것
    - 프레임워크에 비해서 자유롭다
- 프레임워크
    - 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것
    - 폴더명, 파일 명 등에 대한 규칙이 있으며 라이브러리에 비해 엄격하다

## 1.1 디자인 패턴

- 디자인 패턴
    - 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호관계 등을 이용하여 해결할 수 있도록 하나의 ‘규약’ 형태로 만들어 놓은 것을 의미한다
    
    [https://youtu.be/Pzy_MPfGixg](https://youtu.be/Pzy_MPfGixg)
    

### 1.1.1 싱글톤 패턴 Singleton Pattern

- 싱글톤 패턴
    - 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
    - 데이터베이스 연결 모듈에 많이 사용
    - 장점 : 인스턴스를 생성할 때 드는 비용이 줄어든다
    - 단점:  의존성이 높아진다

- Javascript
    
    ```jsx
    
    Class Singleton {
    
    	//생성자
    	constructor(){
    		if(!Singleton.instance){
    				Singleton.instance = this
    		}
    		return Singleton.instance
    	}
    
    	//단일한 instance 반환 모듈
    	getInstance(){
    		return this.instance
    	}
    
    }
    ```
    
- JAVA
    
    *~~배웠던게 끝이 아니라니 당황스럽다. 영상 참고해볼 것..~~* 
    
    [https://youtu.be/3rfbnQYOCFA](https://youtu.be/3rfbnQYOCFA)
    
    [https://youtu.be/4Sk9dzXgKwo](https://youtu.be/4Sk9dzXgKwo)
    

- mongoose
    - mongoose?
        
        > MongoDB는 C++로 작성된 오픈소스 문서지향(Document-Oriented) 적 Cross-platform 데이터베이스이며, 뛰어난 확장성과 성능을 자랑합니다. 또한, 현존하는 NoSQL 데이터베이스 중 인지도 1위를 유지하고있습니다.
        > 
        > 
        > # **NoSQL?**
        > 
        > 흔히 NoSQL이라고 해서 아, SQL이 없는 데이터베이스구나! 라고 생각 할 수도 있겠지만, 진짜 의미는 Not Only SQL 입니다. 기존의 RDBMS의 한계를 극복하기 위해 만들어진 새로운 형태의 데이터저장소 입니다. 관계형 DB가 아니므로, RDMS처럼 고정된 스키마 및 JOIN 이 존재하지 않습니다.
        > 
        > *출처: [https://velopert.com/436](https://velopert.com/436)*
        > 
- MySQL
    - Node.js에서 MySQL 데이터베이스를 연결할 때도 싱글톤 패턴이 쓰인다.
    
    *Node.js익히고 코드 다시 보기*
    
- 싱글톤 패턴의 단점
    - TDD Test Driven Development를 할 때 걸림돌이 된다.
        - Test Driven Development
            
            [Test-Driven Development // Fun TDD Introduction with JavaScript](https://youtu.be/Jv2uxzhPFl4)
            
            [https://youtu.be/Npi21gLIEZM?t=361](https://youtu.be/Npi21gLIEZM?t=361)
            
            - 테스트 주도 개발
                - 테스트 코드를 먼저 개발 → 해당 테스트를 통과할 수 있는 작은 부분의 코드부터 작성 → 새로운 테스트 코드 → 새로운 작은 코드 …
                - 목표, 요구사항에 대한 점검이 가능 ⇒ 사용자 입장에서 코드 작성
                - 구현보다 인터페이스 기반의 개발 가능
    
    - 의존성 주입 DI Dependency Injection
        - 싱글톤 패턴 - 모듈 간의 결합이 강해짐
            
            ⇒ 의존성 주입을 통해 해결 가능
            
            - 의존성 주입
                
                [[DI] 의존성 주입(Dependency Injection) 의 개념과 방법 및 장단점](https://velog.io/@sana/DI-의존성-주입Dependency-Injection-의-개념과-방법)
                
        
        - 메인모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 ‘간접’적으로 의존성을 주입하는 방식이다.
        - 의존성이 떨어진다 = 디커플링 된다.
        - 장점
            - 모듈을 쉽게 교체할 수 있는 구조가 된다
                - 테스팅이 쉬움
                - 마이그레이션하기도 수월함
            - 추상화 레이어를 넣고 이를 기반으로  구현체 넣기
                - 애플리케이션의 의존성 방향 일관적
                - 애플이케이션 쉽게 추론 가능
                - 모듈 간 관계 명확
        - 단점
            - 클래스 수가 늘어나 복잡성 증가 가능
        - 원칙
            - 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 또한, 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.
    

---

### 1.1.2 팩토리 패턴

- 팩토리 패턴
    - 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
    - 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
    - 느슨한 결합
    - 상위클래스 → 인스턴스 생성 방식에 대해 알 필요가 없다
    - 객체 생성 로직 - 별개로 운영
        
        ⇒ 코드를 리팩터링하더라도 한 곳만 고칠 수 있다 = 유지 보수성 증가
        

- Javascript
    
    ```jsx
    class CoffeeFactory {
        static createCoffee(type) {
            const factory = factoryList[type]
            return factory.createCoffee()
        }
    }   
    class Latte {
        constructor() {
            this.name = "latte"
        }
    }
    class Espresso {
        constructor() {
            this.name = "Espresso"
        }
    } 
    
    class LatteFactory extends CoffeeFactory{
        static createCoffee() {
            return new Latte()
        }
    }
    class EspressoFactory extends CoffeeFactory{
        static createCoffee() {
            return new Espresso()
        }
    }
    const factoryList = { LatteFactory, EspressoFactory } 
     
     
    const main = () => {
        // 라떼 커피를 주문한다.  
        const coffee = CoffeeFactory.createCoffee("LatteFactory")  
        // 커피 이름을 부른다.  
        console.log(coffee.name) // latte
    }
    main()
    ```
    
- JAVA
    
    ```java
    enum CoffeeType {
        LATTE,
        ESPRESSO
    }
    
    abstract class Coffee {
        protected String name;
    
        public String getName() {
            return name;
        }
    }
    
    class Latte extends Coffee {
        public Latte() {
            name = "latte";
        }
    }
    
    class Espresso extends Coffee {
        public Espresso() {
            name = "Espresso";
        }
    }
    
    class CoffeeFactory {
        public static Coffee createCoffee(CoffeeType type) {
            switch (type) {
                case LATTE:
                    return new Latte();
                case ESPRESSO:
                    return new Espresso();
                default:
                    throw new IllegalArgumentException("Invalid coffee type: " + type);
            }
        }
    }
    
    public class Main {
        public static void main(String[] args) { 
            Coffee coffee = CoffeeFactory.createCoffee(CoffeeType.LATTE); 
            System.out.println(coffee.getName()); // latte
        }
    }
    ```
    
    - `IllegalArgumentException`
        - 적합하지 않거나(illegal) 적절하지 못한(inappropriate) 인자를 메소드에 넘겨주었을 때 발생
    
- Enum?
    - 상수 집합을 정의할 때 사용되는 타입
    - 자바에서는 다른 언어보다 더 활발히 활용됨
    - 상수 뿐 아니라 메서드를 집어넣어 관리할 수 있다
    - Enum 기반으로 상수 집합 관리 → 코드를 리팩터링 할 때 상수 집합에 대한 로직 수정 시 이 부분만 수정하면 된다는 장점이 있다 *why?*
    - 본질적으로 thread safe하기 때문에 싱글톤 패턴을 만들 때 도움이 된다.
    
    *추가로 더 찾아보기*
    

---

### 1.1.3 전략 패턴 Strategy pattern

- 전략 패턴
    
    = 정책 패턴 policy pattern
    
    - 객체의 행위를 바꾸고 싶은 경우 ‘직접’ 수정하지 않고 전략이라 부르는 ‘캡슐화한 알고리즘’을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하도록 만드는 패턴
    - ex) 결제 방법의 다양성
    
- JAVA
    
    ```java
    import java.text.DecimalFormat;
    import java.util.ArrayList;
    import java.util.List;
    interface PaymentStrategy { 
        public void pay(int amount);
    } 
    
    //카카오카드 클래스 - 전략1
    class KAKAOCardStrategy implements PaymentStrategy {
        private String name;
        private String cardNumber;
        private String cvv;
        private String dateOfExpiry;
        
        public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate){
            this.name=nm;
            this.cardNumber=ccNum;
            this.cvv=cvv;
            this.dateOfExpiry=expiryDate;
        }
    
        @Override
        public void pay(int amount) {
            System.out.println(amount +" paid using KAKAOCard.");
        }
    } 
    
    //루나카드 클래스 - 전략2
    class LUNACardStrategy implements PaymentStrategy {
        private String emailId;
        private String password;
        
        public LUNACardStrategy(String email, String pwd){
            this.emailId=email;
            this.password=pwd;
        }
        
        @Override
        public void pay(int amount) {
            System.out.println(amount + " paid using LUNACard.");
        }
    } 
    
    //공통 클래스
    class Item { 
        private String name;
        private int price; 
        public Item(String name, int cost){
            this.name=name;
            this.price=cost;
        }
    
        public String getName() {
            return name;
        }
    
        public int getPrice() {
            return price;
        }
    } 
    
    class ShoppingCart { 
        List<Item> items;
        
        public ShoppingCart(){
            this.items=new ArrayList<Item>();
        }
        
        public void addItem(Item item){
            this.items.add(item);
        }
        
        public void removeItem(Item item){
            this.items.remove(item);
        }
        
        public int calculateTotal(){
            int sum = 0;
            for(Item item : items){
                sum += item.getPrice();
            }
            return sum;
        }
        
        public void pay(PaymentStrategy paymentMethod){
            int amount = calculateTotal();
            paymentMethod.pay(amount);
        }
    }  
    
    //실행화면
    public class HelloWorld{
        public static void main(String []args){
            ShoppingCart cart = new ShoppingCart();
            
            Item A = new Item("kundolA",100);
            Item B = new Item("kundolB",300);
            
            cart.addItem(A);
            cart.addItem(B);
            
            // pay by LUNACard
            cart.pay(new LUNACardStrategy("kundol@example.com", "pukubababo"));
            // pay by KAKAOBank
            cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789", "123", "12/01"));
        }
    }
    /*
    400 paid using LUNACard.
    400 paid using KAKAOCard.
    */
    ```
    
- passport의 전략 패턴
    - 전략 패턴을 활용한 라이브러리 passport
    - Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리
        - 여러가지 전략을 기반으로 인증 가능함
        - ex) 서비스 내 회원가입 된  ID,PW기반 - LocalStrategy 전략, 네이버, 페이스북 등 다른 서비스 기반 인정 - OAuth 전략
        

---

### 1.1.4 옵저버 패턴 observer pattern

- 옵저버 패턴
    - 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 대마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴
    - 주체 : 객체의 상태 변화를 보고 있는 관찰자
    - 옵저버 : 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 ‘추가 변화 사항’이 생기는 객체를 의미
    - 주체와 객체를 따로 두지 않고, 상태가 변경되는 객체를 기반으로 구축하기도 함
    - 주로 이벤트 기반 시스템에 사용
        - MVC 패턴에도 사용됨
    
- JAVA
    
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
            Topic topic = new Topic(); 
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
    

- Javascript
    
    ```java
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
    
    *이해 안됨*
    
- Vue.js 3.0의 옵저버 패턴
    
    *⇒ Vue.js 공부하고 나서 다시 볼 것*
    

---

### 1.1.5 프록시 패턴과 프록시 서버

- 프록시 객체
    - proxy객체는 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체를 뜻함
    - 자바스크립트 → 프록시 객체는 두 개의 매개 변수를 가짐
        - target : 프록시 할 대상
        - handler : target동작을 가로채고 어떠한 동작을 할 것인지가 설정되어 있는 함수
        
        ```jsx
        const handler ={
        
        	get = function(target, name){
        		return name === 'name' ? `${target.a} ${target.b}` : target[name]
        	}
        }
        
        const p = new Prosy({a: "K", b: "B"}, handler)
        console.log(p.name) //K B
        ```
        
- 프록시 패턴
    - 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴
    - 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사요함
    
    - 프록시 서버에서의 캐싱
        - 캐시 안에 정보를 담아두고, 캐시 안에 있는 정보를 요구하는 요청에 대해 다시 저 멀리 있는 원격 서버에 요청하지 않고, 캐시 안에 있는 데이터를 활용하는 것을 말함
        - 불필요하게 외부와 연결하지 않기 때문에 트래픽을 줄일 수 있다.
    
- 프록시 서버
    - 프록시 서버는 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 가리킴
    - nginx
        - 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹 서버
            
            [Event Loop와 비동기](https://pozafly.github.io/javascript/event-loop-and-async/)
            
            > 
            > 
            > 
            > > Sync : 동기
            > > 
            > 
            > 어떠한 작업이 순차적으로 실행되는 개념이며, 요청을 하면 결과가 반환되는 것을 기다려야 한다.
            > 
            > 동기방식 설계는 매우 직관적이지만, 결과 반환까지 대기해야 하는 시간비효율적인 단점이 있다.
            > 
            > > Async : 비동기
            > > 
            > 
            > 어떠한 작업이 동시에 일어날 수 있는 개념이며, 요청과 결과 반환이 동시에 일어나지 않는다.
            > 
            > 비동기방식 설계는 병렬적으로 작업을 진행하기 때문에 효율적이지만, 설계가 복잡한 단점이 있다.
            > 
            > [https://velog.io/@ongsim123/CS7-카페-주문-이벤트](https://velog.io/@ongsim123/CS7-%EC%B9%B4%ED%8E%98-%EC%A3%BC%EB%AC%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8)
            > 
            
        - Node.js서버를 구축할 때 앞단에 nginx를 두기
        
    - 버퍼 오버플로우
        - 버퍼 : 데이터가 저장되는 메모리 공간
            
            → 이곳을 벗어나는 경우가 버퍼 오버 플로우
            
        - 이때 사용되지 않아야 할 영역에 데이터가 덮어씌워져 주소, 값을 바꾸는 공격이 발생하기도 함
    - gzip압축
        - LZ77과 Huffman 코딩의 조합인 DEFLATE 알고리즘을 기반으로 한 압축 기술
        - 데이터 전송량을 줄일 수 있으나 압축을 해제 햇을 때 서버에서의 CPU 오버헤드도 생각해서 gzip 압축 사용 유무를 결정해야 한다
    
- 프록시 서버로 쓰는 CloudFlare
    - CDN 서비스
        - CDN : 각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크 의미
            
            ⇒ 사용자가 웹 서버로부터 콘텐츠를 다운로드 하는 시간을 줄일 수 있음
            
    - 웹 서버 앞단에 프록시 서버로 두어 DDOS 공격 방어나 HTTPS 구축에 쓰임
    
    - DDOS 공격 방어
    - HTTPS 구축

- CORS와 프런트엔드의 프록시 서버
    - CORS Cross-Origin Resource Sharing
        - 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘
        - 오리진
            - 프로토콜과 호스트 이름, 포트위 조합을 말함
    - 프런트엔드 서버 앞단ㄴ에 프록시 서버를 놓아 해결 가능

---

### 1.1.6 이터레이터 패턴

- 이터레이터 패턴
    - iterator를 사용하여 collection의 요소들에 접근하는 디자인 패턴
    - 순회할 수 있는 여러 가지 자료형의 구조와는 상관 없이, 이터레이터라는 하나의 인터페이스로 순회가 가능함
    
    ```jsx
    const mp = new Map() 
    mp.set('a', 1)
    mp.set('b', 2)
    mp.set('cccc', 3) 
    const st = new Set() 
    st.add(1)
    st.add(2)
    st.add(3) 
    const a = []
    for(let i = 0; i < 10; i++)a.push(i)
    
    for(let aa of a) console.log(aa)
    for(let a of mp) console.log(a)
    for(let a of st) console.log(a) 
    /* 
    a, b, c 
    [ 'a', 1 ]
    [ 'b', 2 ]
    [ 'c', 3 ]
    1
    2
    3
    */
    ```
    
    ⇒ 다른 자료 구조인 set과 map이지만 동일하게 순회 가능
    

---

### 1.1.7 노출모듈 패턴

- 노출모듈 패턴 revealing module pattern
    - 즉시 실행 함수를 통해 private, public과 같은 접근 제어자를 만드는 패턴
    - 자바스크립트는 private, public의 접근 제어자가 존재하지 않음 → 전역 범위에서 실행
        
        ⇒ 노출 모듈 패턴을 통해 구현
        
- javascript
    
    ```jsx
    const pukuba = (() => {
        const a = 1
        const b = () => 2
        const public = {
            c : 2, 
            d : () => 3
        }
        return public 
    })() 
    console.log(pukuba)
    console.log(pukuba.a)
    // { c: 2, d: [Function: d] }
    // undefined
    ```
    
    - a와 b는 다른 모듈에서 사용할 수 없는 변수나 함수이며 private의 범위를 가진다.

---

### 1.1.8 MVC 패턴

- MVC패턴
    - 모델 Model
    - 뷰 View
    - 컨트롤러 Controller

- 모델
    - 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻함
    - 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신
- 뷰
    - 사용자 인터페이스 요소
    - 모델이 가지고 있는 정보를 따로 저장하지 않아야 한다
- 컨트롤러
    - 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할
    - 이벤트 등 메인 로직 담당

- 스프링Spring ⇒ MVC 패턴의 예
    
    [Spring과 MVC패턴 | 디스패처서블릿](https://youtu.be/6ty3GBhqTDM)
    

---

### 1.1.9 MVP 패턴

- MVP 패턴
    - MVC 패턴에서 파생 되었으며 C에 해당하는 컨트롤러가 프레제터로 교체된 패턴
    - 뷰와 프레제터는 일대일 관계 = 따라서 MVC패턴보다 더 강한 결합을 지닌 디자인 패턴이다.

### 1.1.10 MVVM 패턴

- MVVM 패턴
    - MVC의 C 컨트롤러가 뷰모델로 바뀐 패턴
    - 뷰모델 : 뷰를 더 추상화한 계층
    - 커맨드와 데이터 바인딩을 가지는 것이 특징
    - 뷰와 뷰 모델 사이으 ㅣ양방향 데이터 바인딩을 지원
    - UI를 별도의 코드 수정 없이 재사용 가능 - 단위 테스팅 하기 쉬움

- Vue.js
    - 반응형이 특징인 프런트엔드 프레임워크
    - MVVM패턴을 가짐!
    - 함수를 사용하지 않고 값 대입만으로 변수가 변경되어 양방향 바인딩, html을 토대로 컴포넌트를 구축할 수 있다는 점이 특징
    - 재사용 가능한 컴포넌트를 기반으로 UI를 구축할 수 있다.