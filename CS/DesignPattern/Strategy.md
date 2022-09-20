# 전략 패턴(Strategy Pattern)

## 1. 전략 패턴이란?

하나의 클래스가 많은 행동들을 정의하고, 이런 행동들이 그 클래스의 연산 안에서 복잡한 다중 조건문의 모습을 취할 때,
많은 조건문보다는 행동 각각을 **전략(Strategy) 클래스** 로 만들고, 동적으로 행동의 변경이 필요한 경우 **전략(Strategy)** 을 바꾸어 주는 것으로 알고리즘을 다양하게 변경할 수 있게 해주는 패턴

---

## 2. 전략 패턴의 장점

1. 동일 계열의 관련 알고리즘이 생긴다.  
   Strategy 클래스 계층은 동일 계열의 알고리즘군을 정의하고, 알고리즘 자체의 재사용도 가능하게 한다.
2. 알고리즘을 바꾸거나 확장하기 쉬워진다.
3. 조건문을 없앨 수 있다.  
   서로 다른 Strategy 클래스의 행동을 캡슐화하면 조건문을 없앨 수 있다.

---

## 3. 전략 패턴의 단점

1. 객체 수가 증가한다.  
   Strategy들로 생성하는 객체 수가 증가한다.
2. Strategy 객체와 Compostion클래스 객체 사이에 의사소통 오버헤드가 발생할 수 있다.  
   사용하지도 않는 매개변수를 Compostion 객체가 생성하고 초기화하는 경우가 발생할 수 있다.

---

## 4. 간단한 인증방식의 예제

### 4-1. 전체적인 로직

![인증방식 로직](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmYX4w%2FbtqEG94M6sK%2Ff1zfqRSXviDbAwUgLbs3M1%2Fimg.png)

- AuthProgram: 인증진행을 위한 절차를 구현한다.
- AuthStrategy(추상클래스): 제공하는 모든 Auth 알고리즘에 대한 공통의 연산들을 정의한다.
- OAuth, OAuth2, Basic(구체클래스): AuthStrategy 추상클래스를 각각의 실제 Auth 알고리즘으로 구현한다.

AuthProgram에서는 사용하는 각각의 인증방식(OAuth, OAuth2, Basic)이 내부적으로 어떻게 구현 되어있는지 몰라도 된다.
사용자가 선택한 인증전략(AuthStrategy)의 공통된 사용방법(auth()) 만 알고 있으면 된다.

---

### 4-2. 코드를 구현하기

```javascript
class AuthProgram {
  // 인증전력 선택하기
  constructor(authStrategy) {
    this._authStrategy = authStrategy;
  }

  // 선택한 인증전력에 따른 인증 실행
  authenticate() {
    if (!this._authStrategy) {
      console.log("No Authentication Strategy set.");
      return;
    }
    this._authStrategy.auth();
  }
}

// 모든 인증전략 알고리즘이 공통으로 가지고 있는 연산들을 정의
class AuthStrategy {
  auth() {
    throw new Error("auth() must be implement");
  }
}

// 다양한 인증전략 알고리즘
class OAuth extends AuthStrategy {
  auth() {
    console.log("Authenticating useing OAuth Strategy");
  }
}

class OAuth2 extends AuthStrategy {
  auth() {
    console.log("Authenticating useing OAuth2 Strategy");
  }
}

class Basic extends AuthStrategy {
  auth() {
    console.log("Authenticating useing Basic Strategy");
  }
}

const authProgram = new AuthProgram(new OAuth());
authProgram.authenticate();
```

---

## 5. 전략패턴 적용 전과 후의 비교 예시

### 5-1. 전략패턴 적용 전

```javascript
class Strategy {
  constructor() {}
  execute(transport) {
    // if문을 통한 조건문 실행
    if (transport === "ship") {
      console.log("배로 이탈리아에 갑니다.");
    } else if (transport === "land") {
      console.log("육로로 이탈리아에 갑니다.");
    }
  }
}

const transportation = new Strategy();
transportation.execute("ship");
```

---

### 5-2. 전략패턴 적용 후

```javascript
class Strategy {
  constructor(strategy) {
    this._strategy = strategy;
  }
  execute() {
    this._strategy.execute();
  }
}

class Transportation {
  execute() {
    throw new Error("execute() must be implement");
  }
}

class ShipStrategy extends Transportation {
  execute() {
    console.log("배로 이탈리아에 갑니다.");
  }
}

class LandStrategy extends Transportation {
  execute() {
    console.log("육로로 이탈리아에 갑니다.");
  }
}

const transportation = new Strategy(new LandStrategy());
transportation.execute();
```

---

## 참고

[디자인패턴 - 전략 패턴(Strategy Pattern) in Javascript](https://devnest.tistory.com/3)  
[자바스크립트와 전략패턴](https://it-timehacker.tistory.com/78)

---

📅 2022-09-20
