# 싱글톤 패턴(Singleton Pattern)

## 1. 개요

---

## 2. 싱글톤 패턴이란?

전역 변수를 사용하지 않고 **인스턴스(객체)를 하나만 생성** 하도록 하며, 생성된
인스턴스(객체)를 **어디서든지 참조**할 수 있도록 하는 패턴

> 인스턴스란? 설계도를 바탕으로 소프트웨어 세계에 구현된 구체적인 실체

싱글톤 패턴을 적용할 수 있을 만한 상황은 아래와 같다.

- 프로그램 내 에서 어떤 객체가 단 1개만 존재해야 하는 상황
- 프로그램 내부의 여러 부분에서 이 객체를 공유하며 사용해야 하는 상황

즉, 싱글톤 패턴은 프로그램에서 동일한 컨넥션 객체를 생성하거나 단 하나만 존재
해야되는 객체를 생성할 때 유용하게 사용된다.

---

## 3. 싱글톤 패턴을 사용하는 이유(장점)

1.  **메모리 낭비 방지**  
    최초 한 번의 new 연산자를 통해서 고정된 메모리 영역을 사용하기 때문에
    추후 해당 객체에 접근할 때 메모리 낭비를 방지할 수 있다.  
    또한 이미 생성된 인스턴스를 활용하기 때문에 속도 측면에서의 이점도 있다.

2.  **쉬운 데이터 공유**  
    싱글톤 인스턴스는 전역으로 사용되기 때문에 다른 클래스의 인스턴스들이
    접근하여 사용할 수 있다.

---

## 4. 싱글톤 패턴의 문제점

1. **개방-폐쇠 원칙의 위배**  
   싱글톤 인스턴스가 혼자 너무 많은 일을 하거나, 많은 데이터를 공유시키면
   다른 클래스간의 결합도가 높아지게 된다. 이는 **개방-폐쇠 원식**이 위배된다.  
   결합도가 높아지게 되면, 유지보수가 힘들도 테스트도 원할하게 진행할 수 없는 문제점이 발생한다.

   > `개방-폐쇠 원칙`이란 객체지향 5원칙 중 하나로 객체를 다룸에 있어 객체의 확장은
   > 개방적으로, 객체의 수정은 폐쇄적으로 대하는 원칙이다.  
   > [출처](https://blog.itcode.dev/posts/2021/08/14/open-closed-principle)

2. **멀티 스레드 환경에서의 사용**  
   멀티 스레드 환경에서 동기화 처리를 하지 않으면, 싱글톤 인스턴스가
   2개가 생성되는 문제가 발생할 수 있다. 그러므로 멀티 스레드 환경에서
   싱글톤 인스턴스를 사용하기 위해서는 반드시 동기화가 필요하다.

   > `스레드`란 프로세스 내에서 더 작은 단위들로 각각 독립적으로 실행되며,
   > 각각 제어가 가능한 흐름이며 실행 컨텍스트 프로세스라고 함.  
   > [출처](http://www.ktword.co.kr/test/view/view.php?m_temp1=2299)

3. **TDD를 할 때의 걸림돌**  
   TDD(Test Driven Development)를 할 때 단위 테스트를 주로 하게 되는데, 단위 테스트는 테스트가
   서로 독립적이어야 하며 테스트를 어떤 순서대로 실행할 수 있어야 한다. 하지만 싱글톤 패턴은 미리 생성된 하나의
   인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 독립적인 인스턴스를 만들기가 어렵다.

---

## 5. 자바스크립트에서의 싱글톤 패턴

```javascript
class Singleton {
  constructor() {
    // 1) Singleton.instance가 있는 경우 새롭게 생성하지 않고 기존의 Singleton.instance를 반환하기
    if (Singleton.instance) {
      console.warn("Warning: Singleton class alredy instantiated");
      return Singleton.instance;
    }
    // 2) Singleton.instance가 없는 경우 Singleton의 instance에 this를 할당하기
    Singleton.instance = this;

    // 3) 인스턴스의 멤버 변수 초기화하기
    this.version = Date.now();
    this.config = "test";
  }

  // 4) 필요한 메서드 만들기
  update_version() {
    this.version = Date.now();
  }
  get_version() {
    return this.version;
  }
  update_config(value) {
    this.config = value;
  }
  get_config() {
    return this.config;
  }
}
```

`Singleton` 인스턴스를 하나 생성해보자. 그리고 `construcotr` 함수 내에 콘솔로 `Singleton.instance`엔 어떤
값이 담겨 있는지 확인하자.

```javascript
class Singleton {
  constructor() {
    console.log(Singleton.instance);
  // ...
}
const a = new Singleton()
```

인스턴스가 처음 생성될 땐 `Singleton.instance`값이 없기 때문에 `undefined`가 콘솔에 출력된다.

하지만 두 번째 `Singleton` 인스턴스를 생성할 때 부턴 `Singleton.instance`가 존재한다. 아래는
`Singleton.instance`의 값이다.

![Singleton.instance](/image/CS/Singleton/Singleton1.png)

---

### 1) Singleton.instance가 있는 경우 새롭게 생성하지 않고 기존의 Singleton.instance를 반환하기

```javascript
if (Singleton.instance) {
  console.warn("Warning: Singleton class alredy instantiated");
  return Singleton.instance;
}
```

싱글톤 패턴을 가지는 인스턴스는 오직 하나만 존재해야 한다. 그러므로 `new Singleton()` 을 통해
`Singleton` 인스턴스를 생성할 때 이미 생성된 인스턴스가 있는지 확인해야 한다.

이미 생성된 인스턴스가 있다면 간단한 경고 메시지를 보내고 이미 생성된 인스턴스인 `Singleton.instance`를
반환한다.

---

### 2) Singleton.instance가 없는 경우 Singleton의 instance에 this를 할당하기

```javascript
Singleton.instance = this;
```

하지만 이미 생성된 인스턴스가 없다면 즉, 인스턴스가 처음 생성될 땐 `Singleton.instance`에 `this`를
할당한다. 여기서 `this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 변수이다.

---

### 3) 인스턴스의 멤버 변수 초기화하기

```javascript
this.version = Date.now();
this.config = "test";
```

인스턴스의 프로퍼티의 초기값을 설정한다.

---

### 4) 필요한 메서드 만들기

```javascript
class Singleton {
  //...
  update_version() {
    this.version = Date.now();
  }
  get_version() {
    return this.version;
  }
  update_config(value) {
    this.config = value;
  }
  get_config() {
    return this.config;
  }
}
```

필요한 메서드를 만든다.

아래와 같은 상황을 생각해보자.

```javascript
const a = new Singleton();
const b = new Singleton();

a.update_config("develop");
b.get_config();
```

위의 코드에서 마지막 줄엔 어떤 값이 리턴이 될까? `a`, `b` 모두 같은 인스턴스를 가리키고 있으므로
`a`에서 `config`를 업데이트를 했다하더라도 `b`의 `config`도 함께 업데이트가 된다.

콘솔로 확인해보자. 위의 코드 바로 밑에 아래의 코드를 추가 작성하자.

```javascript
console.log(b.get_config());
console.log(a === b);
```

![Singleton](/image/CS/Singleton/Singleton2.png)

첫 번째 콘솔에는 `develop`이 찍히는 것을, 두 번째 콘솔에는 `true`가 찍히는 것을 확인할 수 있다.

---

## 6. mongoose의 싱글톤 패턴

`mongoose`의 데이터베이스를 연결할 때 쓰는 `connect()`라는 함수는 싱글톤 인스턴스를 반환한다.
아래의 티처캔의 데이터베이스를 연결할 때 작성한 코드이다.

```javascript
require("dotenv").config();
import mongoose from "mongoose";

const MONGO_URL = process.env.DATABASE_URL;

const dbConnect = () => {
  mongoose
    .connect(MONGO_URL, {
      dbName: "teachercan-db",
    })
    .then(() => {
      {
        console.log("MongoDB Connected");
      }
    })
    .catch((err) => {
      console.log(err);
    });
};

export default dbConnect;
```

---

## 7. 의존성 주입을 통한 모듈간의 결합을 느슨하게 만들기

---

## 참고

[[Design Pattern] 싱글턴 패턴이란](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)  
[싱글톤 패턴(Singleton pattern)](https://gyoogle.dev/blog/design-pattern/Singleton%20Pattern.html)  
[자바스크립트로 구현한 싱글톤 패턴](https://webruden.tistory.com/171)  
[싱글톤(Singleton) 패턴이란?](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/)  
[싱글톤 패턴이 필요한 이유와 실제 서비스에 적용까지](https://injae-kim.github.io/dev/2020/08/06/singleton-pattern-usage.html)
