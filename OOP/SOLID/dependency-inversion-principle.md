# 의존관계 역전 원칙(Dependency Inversion Principle, DIP)과 추상화
##### Table of Contents
- [의존관계 역전 원칙](#의존관계-역전-원칙)  
- [예제 코드](#예제-코드)
- [추상화란 무엇일까?](#추상화란-무엇일까)
- [추상화의 목적](#추상화의-목적)
    - [얼마나 추상화할 것인가?](#🙋-얼마나-추상화할-것인가)

<br>

### 의존관계 역전 원칙
> 구체화가 아니라 **추상화**에 의존해야 한다.   
> 즉, 구현 클래스(구현체)가 아니라 인터페이스(역할)에 의존해야 한다. 

의존관계 역전 원칙은 `유연하지 못한 코드(Code Rigidity)`, `모듈 간의 강한 결합(Tight Coupling of Modules)`을 피하기 위해 고안되었다.  
의존관계 역전 원칙의 핵심 아이디어 아래와 같다. 

- **고수준 모듈은 저수준 모듈의 구현에 직접 의존해서는 안 된다. 저수준 모듈이 `interface`와 같은 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.**
- **추상화는 세부 사항에 의존해서는 안 되며, 세부 사항(concrete implementations)은 추상화에 의존해야 한다.**

<br>

> #### 고수준 모듈과 저수준 모듈이란? 
> - <small>`고수준 모듈(High-level Module)`: 의미 있는 단일 기능을 제공하는 모듈</small> 
> - <small>`저수준 모듈(low-level Module)`: 고수준 모듈의 기능을 구현하기 위해 필요한 하위 기능의 실제 구현</small> 

### 예제 코드 
> `의존관계 역전 원칙`을 위반하는 모듈 간의 강한 결합(의존성)을 가진 코드를 개선하는 예제를 살펴보자. 

#### As-Is
```js
// LightBulb 클래스
class LightBulb {
    turnOn() {
        console.log('Light bulb turned on.');
    }
}

// Switch 클래스
class Switch {
    constructor() {
        this.bulb = new LightBulb();
    }

    operate() {
        this.bulb.turnOn();
    }
}

// Switch 인스턴스 생성 및 동작
const switchButton = new Switch();
switchButton.operate();
```
해당 코드를 살펴보면, `Switch` 클래스가 `LightBulb` 클래스를 직접 의존하며 모듈 간의 강한 결합이 발생하고 있는 예시이다.   
이후 `LightBulb` 모듈이 아닌 다른 모듈로 교체가 필요할 경우 `Switch` 클래스의 내부 로직도 함께 변경되어야 한다는 문제가 발생한다. 

우리가 원하는 것은 저수준 모듈(`LightBulb`)이 변경되더라도 고수준 모듈(`Switch`)은 변경되지 않는 것이다.  
의존관계 역전 원칙(DIP)을 적용하여 개선한 코드는 아래와 같다. 

#### To-Be
```js
// 인터페이스 정의
class Switchable {
    turnOn() {
        // 공통적인 코드
    }
}

// LightBulb 클래스가 Switchable 인터페이스를 구현
class LightBulb extends Switchable {
    turnOn() {
        console.log('Light bulb turned on.');
    }
}

// Switch 클래스
class Switch {
    constructor(device) {
        this.device = device;
    }

    operate() {
        this.device.turnOn();
    }
}

// LightBulb 인스턴스를 생성하여 Switch 클래스에 주입
const lightBulb = new LightBulb();
const switchButton = new Switch(lightBulb);

// Switch 버튼 동작
switchButton.operate();
```

> 의존 관계를 맺을 때는 **자신보다 변화하기 쉬운 것을 의존해서는 안되고, 거의 변화가 없는 개념에 의존해야한다.** 

변경된 코드를 살펴보면, 이전과 달리 `Switch` 클래스는 `LightBulb` 대신 추상화된 `Switchable` 인터페이스에 의존하고 있다는 점을 확인할 수 있다.   
이를 통해 의존관계 역전 원칙의 **고수준 모듈은 저수준 모듈의 구현에 직접 의존해서는 안 된다...** 를 만족하게 되었다.  

고수준의 모듈(`Switch`)와 저수준의 모듈(`LightBulb`) 두 모듈 간의 느슨한 결합도를 가지도록 개선되었으며
이후 `LightBulb`가 아닌 다른 하위 모듈로의 교체가 필요할 경우에도 `Switch` 내부 로직의 변경이 필요하지 않으며 변화에도 유연하게 대응할 수 있다.   

<br>

### 추상화란 무엇일까?
> 복잡한 내부 프로세스의 로직을 감추고, 간결하고 직관적인 `인터페이스`를 제공하는 것

프로그래밍에서 `추상화(Abstraction)`는 우리가 다루기 어려운 복잡한 대상을 **의도적으로 감춰** 사용자 또는 다른 개발자에게는 명료하고 간결한 인터페이스를 제공하는 중요한 도구이다. 잘 추상화된 기능이나 모듈은 구체적인 구현 세부 사항을 몰라도 쉽게 활용할 수 있도록 만들어준다. 



### 추상화의 목적 
추상화는 단순한 가독성 향상 이상의 목적을 달성할 수 있도록 만들어준다.  
복잡성을 숨김으로써 코드의 `가독성`을 향상시키고 `재사용` 및 `유지보수성`을 높여 개발 과정을 더욱 효율적으로 만들 뿐만 아니라  
모듈화를 통해 `독립성`을 높여 특정 모듈에서 발생한 문제가 전체 시스템에 영향을 미치는 것을 막을 수 있다. 

- 단순화 
- 재사용성 증가
- 유연성 및 확장성 (유지보수)
- 시스템 분리 및 모듈화
- 독립성 증가


### 얼마나 추상화할 것인가?
추상화에 **정답** 이라는 것은 없지만, **상황**과 **목적**에 따라 다르게 적용되어야 한다.  
상황에 따라 필요한 만큼 노출시키기 위해서 **추상화 수준(level of abstraction)** 을 조정하는 과정이 필요하다.
   
덧붙여 `잘못된 추상화보다는 중복이 낫다.`는 말처럼 과도한 추상화는 오히려 코드 속 나쁜 냄새를 만드는 주범일지도 모르겠다.  
그렇기에 반드시 추상화에 앞서, **성급한 추상화**는 아닌지? **상황**과 **목적**에 따라 적절하게 조정되었는지를 염두에 두어야 한다. 
