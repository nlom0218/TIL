# 클래스의 기본

## 1. 개요

클래스를 사용하고자 할 때 기본적으로 이것만은 알고 쓰자. 자세한 내용은 앞으로 점차 공부하며 정리한다.

---

## 2. 클래스란 무엇인가?

클래스(Class)란 객체를 생성하는 일종의 템플릿이다. 인스턴스(클래스로 만들어진 객체)를 찍어내는 공장의 역할을 한다.

---

## 2. 클래스 선언하기

`class 키워드`를 사용하여 클래스를 선언한다. 클래스의 이름은 `파스칼 케이스`를 사용하는 것이 일반적이나 파스칼 케이스를 사용하지 않아도 에러는 발생하지 않는다.

```javascript
class Rectangle {}
```

`Rectangle` 클래스를 생성하였다.

---

## 3. 인스턴스 생성하기

어떤 클래스로 부터 만들어진 객체를 인스턴스라고 한다. 인스턴스를 생성하기 위해서는 `new 연산자`와 함께 클래스를 호출해야 한다.

```javascript
class Rectangle {}

const rectangle = new Rectangle();
```

`rectangle` 인스턴스를 생성하였다.

---

## 4. constructor

`constructor`는 클래스의 특수한 메서드이며 아래의 역할을 수행한다.

1. 인스턴스를 생성한다.
   - 인스턴스를 생성할 때 `new 연산자`와 함께 호출된다.
2. 클래스 필드를 초기화한다.
   - 클래스 필드란 클래스 내부의 캡슐화된 변수이다.
   - constructor의 파라미터에 전달한 값은 클래스 필드에 할당하면서 초기화를 할 수 있다.

이러한 `constructor`는 클래스 내에서 생략을 할 수 있지만 존재할 경우 한 개만 존재할 수 있다. 만약 생략될 경우 클래스에 `constructor() {}`를 포함한 것과 동일하게 동작한다.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
}

const rectangle = new Rectangle(10, 20);
```

---

## 5. Class field declarations proposal

```javascript
class Rectangle {
  width;
  height;

  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
}

const rectangle = new Rectangle(10, 20);
```

`constructor` 의 위쪽에 선언한 `width`와 `height`가 클래스 필드에 해당한다. 기존엔 위와 같이 클래스 필드를 선언하면 오류가 발생하였지만 `Class field declarations proposal`으로 인해 정상 동작한다. 아래 주소 참고

[Class field declarations for JavaScript](https://github.com/tc39/proposal-class-fields)

그러다면 클래스 외부에서 접근할 수 없는 `Private`한 클래스 필드를 만들기 위해선 어떻게 해야 할까? 위의 예제를 아래와 같이 수정하자.

```javascript
class Rectangle {
  #width;
  #height;

  constructor(width, height) {
    this.#width = width;
    this.#height = height;
  }
}

const rectangle = new Rectangle(10, 20);

const rectangleWidth = rectangle.#width;
```

위 `rectangleWidth`에 과연 접근할 수 있을까? 정답은 아니다. 아래와 같은 문법 오류가 나타난다.

![SyntaxError](/image/JS/Class/BasicClass/SyntaxError1.png)

즉, `#width`는 `Private field`이기 때문에 클래스 외부에서 접근할 수 없다.
이와 같이 클래스 외부에서 접근할 수 없는 클래스 필드를 만드려면 `#` 를 사용하여 `Private`하게 만들자.

---

## 6. 메서드

클래스 내부에 메서드를 정의하여 인스턴스를 통해 호출할 수 있다.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getRound() {
    return (this.width + this.height) * 2;
  }

  getArea() {
    return this.width * this.height;
  }
}

const rectangle = new Rectangle(10, 20);

rectangle.getRound(); // 60
rectangle.getArea(); // 200
```

사각형의 둘레를 구하는 `getRound()` 메서드, 사각형의 넓이를 구하는 `getArea()` 메서드를 정의한 후 `rectangle` 인스턴스를 통해 호출하였다. 메서드에서 사용된 `this`는 인스턴스 자체를 의미한다. 즉 `rectangle`을 의미한다.

---

## 7. 정적 메서드

클래스의 정적 메서드를 정의할 땐 `static` 키워드를 사용한다. 정적 메서드는 일반 메서드와 달리 인스턴스를 통해 호출하지 않고 `클래스 이름`으로 호출한다. 이것은 정적 메소드는 `this`를 사용할 수 없다는 것을 의미한다.

사각형인지 확인하는 정적 메서드를 만들어보자.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getRound() {
    return (this.width + this.height) * 2;
  }

  getArea() {
    return this.width * this.height;
  }

  static isRectangle({ width, height }) {
    if (!width || !height) return false;

    return true;
  }
}

const rectangle = new Rectangle(10, 20);

Rectangle.isRectangle(rectangle); // true
Rectangle.isRectangle({ width: 10 }); // false
```

클래스 내부의 `static isRectangle()` 메서드가 정적 메서드이다. 물론 사각형인지 확인하는 과정은 더 복잡하지만 일단은 간단히만 정리하였다.

---

## 8. Conclusion

> 클래스를 다루게 된 것은 비교적 최근이다. 이번 여름, 자료 구조를 공부할 때 연결리스트, 그래프 등의 구현을 위해 사용했었다. 그래서 아직 클래스에 대한 개념이 부족하다.  
> 이번 챕터에서는 처음부터 너무 깊게 파고 들지 말고 처음엔 간단하게 사용할 수 있는 방법 위주로 정리를 하였다. 이후엔 상속, this, super 등에 대한 내용을 정리하고자 한다.

---

## 참고

[poiemaweb - 클래스](https://poiemaweb.com/es6-class)

---

📅 2022-11-12
