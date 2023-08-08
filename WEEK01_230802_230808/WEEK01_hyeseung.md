## 디자인 패턴

- 리액트, 뷰, 스프링 등 라이브러리나 프레임워크의 기본이 된다.
    
    <aside>
        
        💡 라이브러리와 프레임워크
        공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것임은 같지만, 라이브러리는 비교적 자유롭고 내가 직접 컨트롤 해서 사용한다. 반면 프레임워크는 규칙이 좀 더 엄격하고 컨트롤의 주체가 프레임워크이다.
    
    </aside>
    
- 프로그램을 설계할 때 발생했던 문제점들을 객체 간 상호관계 등을 이용하여 해결할 수 있도록 규약 형태로 만들어 놓은 것

## 싱글톤 패턴

### 싱글톤 패턴이란?

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
    - 하나의 인스턴스 만들어 놓고 다른 모듈들이 공유하여 사용
- 보통 데이터베이스 연결 모듈에 많이 사용
    - DB.instance 라는 하나의 인스턴스 기반으로 a,b 생성 → 비용 절약
        
        ```java
        // DB 연결을 하는 것이기 때문에 비용이 더 높은 작업 
        const URL = 'mongodb://localhost:27017/kundolapp' 
        const createConnection = url => ({"url" : url})    
        class DB {
            constructor(url) {
                if (!DB.instance) { 
                    DB.instance = createConnection(url)
                }
                return DB.instance
            }
            connect() {
                return this.instance
            }
        }
        const a = new DB(URL)
        const b = new DB(URL) 
        console.log(a === b) // true
        ```
        

- 장단점?
    - 인스턴스 생성 비용이 줄어든다는 장점
    - 의존성이 높아진다는 단점

### 자바스크립트

- 자스 특성상 객체 생성시 다른 어떤 객체와도 같지 않게 만들어진다.
    
    (= 자체적으로 어느정도 싱글톤 패턴 구현됨)
    
    ```java
    const obj = {
        a: 27
    }
    const obj2 = {
        a: 27
    }
    console.log(obj === obj2)
    // false
    ```
    

- 실제 구현 코드
    
    ```java
    class Singleton {
        constructor() {
            if (!Singleton.instance) { // 기존 인스턴스 없는 경우
                Singleton.instance = this
            }
            return Singleton.instance
        }
        getInstance() {
            return this 
        }
    }
    const a = new Singleton()
    const b = new Singleton() 
    console.log(a === b) // true
    ```
    

### 자바

- 구현 코드
    
    ```java
    class Singleton {
        private static class singleInstanceHolder {
            private static final Singleton INSTANCE = new Singleton();
    				// final 키워드를 통해서 read only 즉, 다시 값이 할당되지 않도록 했습니다.
        }
        public static Singleton getInstance() {
            return singleInstanceHolder.INSTANCE;
        }
    }
    
    public class HelloWorld{ 
         public static void main(String []args){ 
            Singleton a = Singleton.getInstance(); 
            Singleton b = Singleton.getInstance(); 
            System.out.println(a.hashCode());
            System.out.println(b.hashCode());  
            if (a == b){
             System.out.println(true); 
            } 
         }
    }
    /*
    705927765
    705927765
    true
    */
    ```
    
- (필자의) 주석
    - 클래스안에 클래스(Holder), static이며 중첩된 클래스인 singleInstanceHolder를 기반으로 객체를 선언했기 때문에 한 번만 로드되므로 싱글톤 클래스의 인스턴스는 애플리케이션 당 하나만 존재하며 클래스가 두 번 로드되지 않기 때문에 두 스레드가 동일한 JVM에서 2개의 인스턴스를 생성할 수 없습니다. 그렇기 때문에 동기화, 즉 synchronized를 신경쓰지 않아도 됩니다.
    - 중첩클래스 Holder로 만들었기 때문에 싱글톤 클래스가 로드될 때 클래스가 메모리에 로드되지 않고 어떠한 모듈에서 getInstance()메서드가 호출할 때 싱글톤 객체를 최초로 생성 및 리턴하게 됩니다.

### mongoose 모듈

Node.js에서 MongoDB 데이터베이스를 연결할 때 쓰는 모듈

- 데이터베이스 연결할 때 쓰는 `connect()` → 싱글톤 인스턴스 반환
- connect() 함수 구현 코드
    
    ```java
    Mongoose.prototype.connect = function (uri, options, callback) {
        const _mongoose = this instanceof Mongoose ? this : mongoose;
        const conn = _mongoose.connection;
        return _mongoose._promiseOrCallback(callback, cb => {
            conn.openUri(uri, options, err => {
                if (err != null) {
                    return cb(err);
                }
                return cb(null, _mongoose);
            });
        });
    };
    ```
    

### MySQL

Node.js에서 MySQL 연결할 때도 싱글톤 패턴 사용

- 구현 코드

```java
// 메인 모듈
// db 연결에 관한 인스턴스 정의
const mysql = require('mysql');
const pool = mysql.createPool({
    connectionLimit: 10,
    host: 'example.org',
    user: 'kundol',
    password: 'secret',
    database: '승철이디비'
});
pool.connect();

// pool 인스턴스 기반으로 쿼리 보냄
// 모듈 A
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});
// 모듈 B
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});
```

### 단점 및 해결방안

- TDD(Test Driven Development) 를 할 때 문제 발생
    - 단위 테스트의 전제조건은 서로 독립적인 테스트여야 하는 것
    - 싱글톤 패턴은 하나의 인스턴스를 기반으로 하므로 각 테스트마다 독립적인 인스턴스 만들기가 어려움
- 모듈 간 결합을 강하게 만들 수 있다. → 의존성 주입으로 해결 가능

- 의존성 주입(DI)
    
    <aside>

        💡 의존성(종속성)
        e.g. A가 B에 의존성이 있다 = B의 변경사항에 대해 A도 변해야 한다.
    
    </aside>
    
    - 모듈 간 결합을 느슨하게 해준다.
    - 메인모듈이 직접 하위 모듈들에 의존성을 주지 않고, 의존성 주입자가 중간에서 가로채 메인모듈이 간접적으로 의존성을 주도록 한다.

- 의존성 주입의 장점
    - 의존성이 감소하는 (디커플링) 효과가 있다.
    - 모듈들을 쉽게 교체할 수 있어 테스팅 및 마이그레이션이 수월
    - 구현시 추상화 레이어를 넣고 그것을 기반으로 구현체를 넣어주기 때문에, 애플리케이션 의존성 방향이 일관되고 쉽게 추론이 가능하며 모듈간 관계가 명확해진다.
- 의존성 주입의 단점
    - 모듈들이 분리되기 때문에 클래스 수가 늘어나 복잡성이 증가할 가능성이 있다.
    - 약간의 런타임 페널티

<aside>

    💡 의존성 주입 원칙

    상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
    둘 다(상위, 하위 모듈) 추상화에 의존해야 한다. 
    이 때 추상화는 세부 사항에 의존하지 말아야 한다.

</aside>

## 팩토리 패턴

### 팩토리 패턴이란?

- 객체 생성 부분을 떼어내 추상화한 패턴
- 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴

- 장점
    - 상위 클래스와 하위 클래스가 분리되는 특성 → 느슨한 결합
    - 상위 클래스에서 더 많은 유연성 갖게 됨
    - 유지 보수성의 증가 → 객체 생성 로직은 별도이기 때문에, 코드를 리팩터링하더라도 한 곳만 고치면 됨

- 예시
    - 바리스타 공장(상위 클래스) > 라떼 레시피, 아메리카노 레시피, 우유 레시피(하위 클래스)
    - 바리스타 공장에서 특정 메뉴를 생산하는 공정을 생각해라!

### 자바스크립트

- 예제
    
    ```java
    const num = new Object(42) // 팩토리 패턴 구현
    const str = new Object('abc')
    num.constructor.name; // Number
    str.constructor.name; // String
    ```
    
    - 어떤 것을 전달하느냐에 따라 다른 타입의 객체를 생성해준다.
- 구현 코드
    
    ```java
    class CoffeeFactory { // 상위 클래스
        static createCoffee(type) { // static으로 선언하면 클래스 내에 함수 정의 가능
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
    
    class LatteFactory extends CoffeeFactory{ // 상속 받은 하위 클래스
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
    
    - 해당 코드는 의존성 주입이라고도 볼 수 있다.
        - CoffeeFactory에서 직접 인스턴스 생성x
        - LatteFactory 에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문

### 자바

- 구현 코드
    
    ```java
    enum CoffeeType { // 상수의 집합 정의할 때 사용하는 타입
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
    			// 교재의 if문 대신 enum 사용 (매핑)
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
    
    - Enum 타입의 장점
        - 코드 리팩터링할 때 해당 집합에 관한 로직 수정 시 해당 부분만 수정하면 된다.
    

## 전략 패턴

### 전략 패턴이란?

- = 정책 패턴
- 객체의 행위를 바꾸고 싶은 경우 직접 수정 x
- 캡슐화한 알고리즘을 **컨텍스트** 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴

<aside>

    💡 컨텍스트 : 
    상황, 맥락, 문맥 / 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보

</aside>

*e.g. 결제 방식의 “전략” 만 바꿔서 두 가지 방식으로 결제하는 것*

### 자바

- 구현 코드
    
    ```java
    import java.text.DecimalFormat;
    import java.util.ArrayList;
    import java.util.List;
    interface PaymentStrategy { // 인터페이스
        public void pay(int amount);
    } 
    
    class KAKAOCardStrategy implements PaymentStrategy { // 구현체
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
    
    class LUNACardStrategy implements PaymentStrategy { // 구현체
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
    

### passport

- 전략 패턴을 활용한 라이브러리
- 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리

- 구현 코드 - LocalStrategy 인증 전략 사용해서 인증하기

```java
var passport = require('passport')
    , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy( // use() 메서드에 "전략"을 매개변수로 넣음
    function(username, password, done) {
        User.findOne({ username: username }, function (err, user) {
          if (err) { return done(err); }
            if (!user) {
                return done(null, false, { message: 'Incorrect username.' });
            }
            if (!user.validPassword(password)) {
                return done(null, false, { message: 'Incorrect password.' });
            }
            return done(null, user);
        });
    }
));
```

## 옵저버 패턴

### 옵저버 패턴이란?

- 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 옵저버들에게 변화 알려주는 디자인 패턴
    
    <aside>

        💡 주체와 옵저버
        주체 = 객체의 상태 변화 관찰자
        옵저버 = 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 추가 변화 사항이 생기는 객체
    
    </aside>
    

- 객체와 주체가 분리되기도, 합쳐지기도 함
    - 합쳐지는 경우, 상태가 변경되는 객체를 기반으로 구축한 것

- 사용 사례
    - 트위터
    - 주로 이벤트 기반 시스템이나 **MVC 패턴**에 사용
        - 모델(주체)에 변경 사항이 생기면 `update()` 메서드로 뷰(옵저버)에 알려주고, 이를 기반으로 컨트롤러가 작동

### 자바

- 구현 코드
    
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
    
    class Topic implements Subject { // 토픽(주체이자 객체) 기반으로 옵저버 패턴 구현
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
            this.observers.forEach(Observer::update); // 업데이트
        }
    
        @Override
        public Object getUpdate(Observer obj) {
            return this.message;
        } 
        
        public void postMessage(String msg) { // 객체가 메시지 올리면
            System.out.println("Message sended to Topic: " + msg);
            this.message = msg; 
            notifyObservers(); // 옵저버에게 알림
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
            Observer a = new TopicSubscriber("a", topic); // 옵저버 선언시 이름, 토픽 지정해야 한다.
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
    

### 자바 - 상속과 구현

- 상속
    - 자식 클래스가 부모 클래스의 메서드 등 상속받아 추가 및 확장
    - 장점 : 재사용성, 중복성의 최소화
    - 일반 클래스, 추상 클래스 기반으로 구현
- 구현
    - 부모 인터페이스를 자식 클래스에서 재정의해서 구현
    - 상속과의 차이점 : 부모 클래스의 메서드 반드시 재정의 해야한다.
    - 인터페이스 기반으로 구현

### 자바스크립트

자스의 옵저버 패턴은 프록시 객체를 통해 구현한다.

<aside>

    💡 프록시 객체
    어떠한 대상의 기본적인 동작의 작업을 가로챌 수 있는 객체
    두 개의 매개변수 가진다.
    *target : 프록시할 대상
    *handler : 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수

</aside>

- 구현 코드 - 프록시 객체
    
    ```java
    const handler = {
    	get: function(target, name) {
    		return name === 'name'? `${target.a} ${target.b}` : target[name]
    	}
    }
    
    const p = new Proxy({a: 'KUNDOL', b: 'IS AUMUMU ZANGIN'}, handler)
    console.log(p.name) // KUNDOL IS AUMUMU ZANGIN
    ```
    
    - handler 에 입력한 것 → name 속성에 접근할 때는 a와 b를 합쳐서 문자열을 만들어라!
    - name 속성을 선언하지 않아도 p.name을 통해 name에 접근하려고 하면 문자열 반환해줌
    
- 구현 코드 - 옵저버 패턴(프록시 객체 이용)
    
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
    
- 프록시 객체의 함수들
    - get() : 속성과 함수에 대한 접근 가로챔
    - has() : in 연산자의 사용 가로챔
    - set() : 속성에 대한 접근을 가로챔
        - 위 코드에서 접근을 가로채서 속성이 변화(솔로 → 커플)하는 것을 감시함
    

### Vue.js 3.0

- ref 나 reactive로 정의하면 해당 값이 변경되었을 때 자동으로 DOM 에 있는 값이 변경됨
    → 프록시 객체를 이용한 옵저버 패턴을 이용해서 구현한 것
    
- 구현 코드
    
    ```java
    function createReactiveObject(
        target: Target,
        isReadonly: boolean,
        baseHandlers: ProxyHandler < any > ,
        collectionHandlers: ProxyHandler < any > ,
        proxyMap: WeakMap < Target, any > // 프록시 객체
    ) {
        if (!isObject(target)) {
            if (__DEV__) {
                console.warn(`value cannot be made reactive: ${String(target)}`)
            }
            return target
        }
        // target is already a Proxy, return it.
        // exception: calling readonly() on a reactive object
        if (
            target[ReactiveFlags.RAW] &&
            !(isReadonly && target[ReactiveFlags.IS_REACTIVE])
        ) {
            return target
        }
        // target already has corresponding Proxy
        const existingProxy = proxyMap.get(target) // get() 함수
        if (existingProxy) {
            return existingProxy
        }
        // only a whitelist of value types can be observed.
        const targetType = getTargetType(target)
        if (targetType === TargetType.INVALID) {
            return target
        }
        const proxy = new Proxy(
            target,
            targetType === TargetType.COLLECTION ? collectionHandlers :
            baseHandlers
        )
        proxyMap.set(target, proxy) // set() 함수
        return proxy
    }
    ```
    

## 프록시 패턴과 프록시 서버

### 프록시 패턴

- 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 앞단의 인터페이스 역할을 하는 디자인 패턴
- 객체의 속성, 변환 등을 보완
    - 보안, 데이터 검증, 캐싱, 로깅 등에 사용
    - 앞의 프록시 객체 역시 디자인 패턴 중 하나인 프록시 패턴이 녹아들어 있는 객체
    - 프록시 서버로도 활용됨

<aside>

    💡 캐싱
    캐시 안에 정보를 담아두고, 캐시 안의 정보를 요구하는 요청을 만나면 원격 서버에 요청하기보다 캐시 안의 데이터를 활용하는 것
    트래픽을 줄일 수 있다는 장점이 있다.

</aside>

### 프록시 서버

- 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램
- nginx
    - 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리할 수 있는 웹서버
    - 주로 Node.js 서버 프론트엔드 프록시 서버로 활용된다.
        - 버퍼 오버플로우 예방
        - 간접적으로 한 단 계 더 거치게 함으로써 익명 사용자의 직접적인 접근 차단 및 보안성 강화
        - 실제 포트를 숨길 수 있고, 정적 자원을 gzip 압축하거나, 로깅을 할 수도 있다.
        
        <aside>

            ✏️ gzip 압축
            DEFLATE 알고리즘을 기반으로 한 압축 기술
            데이터 전송량을 줄일 수 있음 + 하지만 압축 해제 시 서버 CPU 오버헤드도 고려해서 사용을 결정해야 한다.
        
        </aside>
        

- CloudFlare
    - 전 세계적으로 분산된 서버를 기반으로 시스템의 콘텐츠 전달 빠르게 할 수 있는 **CDN 서비스**
        - 사용자가 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 및 배포하는 서버 네트워크
    - 뿐만 아니라 디도스 공격 방어, HTTPS 구축 기능도 있다.
        
        → 위 항목들은 CloudFlare 서비스를 웹 서버의 프록시 서버로 사용해서 오는 이점들
        
    
    - 디도스 공격 방어
        - 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 디도스 공격
        - CloudFlare는 의심스러운 트래픽 자동으로 차단 + 방화벽 대시보드 제공
        - 거대한 네트워크 용량과 캐싱 전략 → 소규모 공격은 쉽게 막을 수 있음
    - HTTPS 구축
        - 별도의 인증서 설치 없이 구축 가능

- CORS와 프론트엔드의 프록시 서버
    - CORS : 서버가 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘
        - 오리진 = 프로토콜 + 호스트 이름 + 포트
    - CORS 에러 해결하기 위해 앞단에 프록시 서버 만들기도 한다.
        - e.g. 테스팅 할 때 서버와 포트 번호 다른 경우 → 프록시 서버 이용해서 오리진을 수정
        - 뿐만 아니라 다양한 API 서버와의 통신도 매끄럽게 한다.

## 이터레이터 패턴

### 이터레이터 패턴이란?

이터레이터를 사용하여 컬렉션 요소들에 접근하는 디자인 패턴
자료형의 구조와 상관없이 “이터레이터” 라는 하나의 인터페이스를 사용하여 순회 가능 

### 자바스크립트

- 구현 코드
    
    ```java
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
    
    for(let aa of a) console.log(aa) // 이터레이터 프로토콜(규칙)
    for(let a of mp) console.log(a) // -> for a of b 형태
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
    

## 노출모듈 패턴

### 노출모듈 패턴이란?

- 즉시 실행 함수를 통해 접근 제어자를 만드는 패턴
    - 함수를 정의하자마자 바로 호출하는 함수
    - 초기화 코드, 전역 변수 충돌 방지 등에 사용

- 자바스크립트는 일반적으로 접근 제어자 x, 전역 범위에서 실행
    → 노출모듈 패턴으로 따로 접근 제어자 구현
    → 모듈 방식 중 하나인 CJS(CommomJS)도 노출모듈 패턴 기반으로 만듦
    

### 자바스크립트

- 구현 코드
    
    ```java
    const pukuba = (() => {
        const a = 1
        const b = () => 2 // private 범위
        const public = {
            c : 2, 
            d : () => 3 // public 범위
        }
        return public 
    })() 
    console.log(pukuba)
    console.log(pukuba.a)
    // { c: 2, d: [Function: d] }
    // undefined -> private이라 안뜸
    ```
    

## MVC 패턴

*모델(Model) + 뷰(View) + 컨트롤러(Controller)*

### 왜 나눌까?

- 애플리케이션의 구성 요소를 구분하였기 때문에, 각각 구성 요소에만 집중해서 개발이 가능
- 재사용성과 확장성이 용이
- 단점) 애플리케이션이 복잡해지면 모델과 뷰의 관계도 복잡해진다.

### 모델

- 애플리케이션의 데이터
- 데이터베이스, 상수, 변수 등

- 뷰에서 데이터를 생성하거나 수정하면,
- 컨트롤러를 통해 **모델**을 생성하거나 갱신한다.

### 뷰

- 사용자 인터페이스(UI) 요소
- 화면에 표시되는 정보만 가지고 있어야 한다.

- 변경이 일어나면 컨트롤러에 전달

### 컨트롤러

- 모델과 뷰를 잇는 다리 역할
- 이벤트 등 메인 로직 담당
- 뷰의 생명 주기 관리

- 모델이나 뷰가 업데이트되면 이를 해석하여 각각 구성요소에 전달

### MVC 패턴의 예 - React.js

- 유저 인터페이스를 구축하기 위한 라이브러리
- 가상 DOM을 활용하여 실제 DOM을 조작하는 것을 추상화함 → 성능 향상

- 특성
    - 불변성 : setState를 통해서만 수정 가능, pureComponent(props 기반으로 생성)
    - 단방향 바인딩
    - 높은 자유도

## MVP 패턴

*모델(Model) + 뷰(View) + 프레젠터(Presenter)*

- MVC와의 차이?
    - 뷰와 프레젠터는 일대일 관계
    - MVC보다 더 강한 결합을 가짐

## MVVM 패턴

*모델(Model) + 뷰(View) + 뷰모델(View Model)*

- 뷰모델 = 뷰를 더 추상화한 계층

- MVC와의 차이?
    - 커맨드, 데이터 바인딩 가진다
        - 커맨드 : 여러 가지 요소에 대한 처리를 하나의 액션으로 처리
        - 데이터 바인딩 : 화면의 데이터와 메모리 데이터 일치 → 뷰모델 변경시 뷰도 변경된다.

- 특징
    - 뷰와 뷰모델 간 양방향 데이터 바인딩 지원
    - UI 재사용 가능
    - 단위 테스팅 용이

### MVVM 패턴의 예 - Vue.js

- 반응형이 특징인 프론트엔드 프레임워크

- 특징
    - 함수 사용하지 않고 값만 대입해도 변수가 변경됨
    - 양방향 바인딩, html 토대로 컴포넌트 구축 가능
    - 재사용 가능한 컴포넌트 기반으로 UI 구축 가능