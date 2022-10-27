# Class

## 1. 개요

자바스크립트는 **프로토타입** 기반의 언어이므로 클래스의 개념이 존재하지 않는다. 하지만 프로토타입을 일반적인 의미에서의 클래스 관점에서 접근해보면 비슷하게 해석할 수 있다.

---

## 2. 자바스크립트의 클래스

생성자 함수 Array를 new 연산자와 함께 호출하면 인스턴스가 생성된다. 이때 Array 내부 프로퍼티들 중 prototype 프로퍼티를 제외한 나머지는 인스턴스에 상속되지 않는다. 이런 여부에 따라 스택티 멤버(static member)와 인스턴스 멤버(instance member)로 나뉜다. 인스턴스 메서드드는 자바스크립트의 특징을 살려 프로토타입 메서드(prototype method)라고 부를 수 있다.

예를 들어 자바스크립트에서의 `Array`와 관련된 메서드는 아래와 같이 나뉠 수 있다.

1. static methods
   - from()
   - isArray()
   - of()
2. prototype methods
   - push()
   - pop()
   - forEach()x
   - map()
   - 등등

어떻게 사용하는지 아래의 코드를 통해 살펴보자.

```javascript
const arr = [1, 2, 3];
Array.isArray(arr);
arr.isArray();
```

위의 코드에서 어떤 결과값이 예상이 되는가?

`Array.isArray(arr)`는 `true`가 반환된고 `arr.isArray()`는 아래와 같은 오류가 발생한다.

![isArray() Error](/image/JS/Class/isArrayError.png)

이번엔 아래의 코드를 살펴보자.

```javascript
const arr = [1, 2, 3];
Array.pop();
arr.pop();
```

`Array.pop()`는 아래와 같은 오류가 발생하고 `arr.pop()`은 `arr` 배열의 마지막 요소인 `3`를 반환횐다.

![pop() Error](/image/JS/Class/popError.png)

이를 정리하자면 `static methods`는 인스턴스에 사용될 수 없고 `prototype methods`는 인스턴스에 사용될 수 있다.

---

### 2-1. 클래스 관점에서 바라본 프로토타입의 시스템

```javascript
let Rectangle = function (width, height) {
  this.width = width;
  this.height = height;
};

Rectangle.prototype.getArea = function () {
  return this.width * this.height;
};

Rectangle.isRectangle = (instance) => {
  return (
    instance instanceof Rectangle && instance.width > 0 && instance.height > 0
  );
};

const rect = new Rectangle(3, 4);
rect.getArea();
rect.isRectangle();
Rectangle.isRectangle(rect);
```

위의 결과는 어떨까? `rect.getArea()`는 12를 반환하고 `Rectangle.isRectangle(rect)`은 true를 반환한다.

하지만 `rect.isRectangle()`은 아래와 같은 에러를 발생한다. 그 이유는 우선 `rect`에 해당 메서드가 있는지 검색했는데 없고, `rect.__proto__`에서도 없으며, `rect.__proto__.__proto__(= Object.prototype)`에도 없기 때문이다.

![isRectangle() Error](/image/JS/Class/isRectangleError.png)

이렇게 인스턴스에서 직접 접근할 수 없는 메서드를 스텍티 메서드라고 하고 접근할 수 있는 메서드를 프로토타입 메서드라고 한다.

---

## 참고

도서 - 코어 자바스크립트
