

# 싱글톤 패턴

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 보통 데이터베이스 연결 모듈에 많이 사용
- 장점
    - 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어듦
- 단점
    - 의존성이 높아짐

## 자바스크립트의 싱글톤 패턴

```jsx
class Singleton {
    constructor() {
        if (!Singleton.instance) {
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

## 자바에서의 싱글톤 패턴

```java
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}
```

## 단점

- TDD(Test Driven Development)를 할 때 단위 테스트를 주로 하는데, 각 테스트마다 ‘독립적인’ 인스턴스를 만들기가 어려움
- 모듈 간의 결합을 강하게 만들 수 있다
    - 의존성 주입을 통해 해결 가능

## 의존성 주입(DI, Dependency Injection)

> 💡 의존성(종속성)
> 
> - A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미

메인 모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식

⇒ 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 됨(디커플링)

### 장단점

- 장점
    - 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기 수월
    - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 어플리케이션 의존성 방향이 일관 되고, 어플리케이션을 쉽게 추론할 수 있으며, 모듈간의 관계들이 조금 더 명확해짐
- 단점
    - 모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성 증가
    - 약간의 런타임 패널티

### 원칙

- 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 함
- 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 함