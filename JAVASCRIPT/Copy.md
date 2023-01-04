# 얕은 복사와 깊은 복사

## 1. 개요

값을 복사하기 위한 방법인 얕은 복사와 깊은 복사를 정리한다.

---

## 2. 얕은 복사(Shallow copy)

얕은 복사은 객체의 참조값(주소 값)을 복사한다. 값을 복사한다 하더라도, 인스턴스가 메모리에 새로 생성되지 않는다. 값 자체를 복사하는 것이 아니라 주소값을 복사하여 같은 메모리를 가리키기 때문이다.

얕은 복사는 아래와 같이 두 가지의 경우로 나눌 수 있다.

1. 객체를 할당한 변수를 다른 변수에 할당하는 것
2. 객체를 프로퍼티 값으로 갖는 객체의 경우 한 단계까지만 복사하는 것

### 2-1. 객체를 할당한 변수를 다른 변수에 할당

바로 코드로 살펴보자.

```javascript
const origin = { name: 'HD' };
const copy = origin;

console.log(origin === copy); // true

copy.name = 'KH';
copy.age = 30;

console.log(origin); // {name: 'KH', age: 30}
console.log(copy); // {name: 'KH', age: 30}
```

`origin` 객체와 `copy` 객체는 같은 참조값(주소 값) 가지고 있어 같은 메모리를 가리키고 있다. 즉, 하나의 메모리의 주소를 서로 가지고 있다고 생각하면 된다.

이는 객체의 프로퍼티를 수정한다면 서로에게 영향을 준다.

아래는 또 다른 예시이다.

```javascript
const origin = [0, 1, 2, 3];
const copy = origin;

console.log(origin === copy); // true

copy[0] = -1;
copy.push(4);

console.log(origin); // [-1, 1, 2, 3, 4]
console.log(copy); // [-1, 1, 2, 3, 4]
```

### 2-2. 객체를 프로퍼티 값으로 갖는 객체의 경우 한 단계까지만 복사

코드로 살펴보자.

```javascript
const origin = {
  name: 'HD',
  fav: {
    food: 'pizza',
  },
};
const copy = { ...origin };

console.log(origin === copy); // false
console.log(origin.fav === copy.fav); // true
```

`origin` 객체는 `fav` 객체를 프로퍼티를 가지고 있다. `copy` 객체는 이러한 `origin` 객체를 스프레드 문법으로 복사를 하였다. 이렇게 된다면 각각의 객체는 서로 다른 메모리의 주소를 가지고 있지만 `fav` 프로퍼티에 대해서는 같은 메모리 주소를 가지고 있다.

이번에는 객체의 프로퍼티를 변경해보자.

```javascript
copy.name = 'KH';

console.log(origin); // {name: "HD", fav: {food: "pizza"}}
console.log(copy); // {name: "KH", fav: {food: "pizza"}}
```

`name` 프로퍼티의 값을 바꾸어도 원본 객체에는 영향을 주지 않는 것을 확인할 수 있다. 그렇다면 객체 프로퍼티인 `fav` 객체의 프로퍼티를 변경하면 어떨까?

```javascript
copy.name = 'KH';
copy.fav.food = 'beef';

console.log(origin); // {name: "HD", fav: {food: "beef"}}
console.log(copy); // {name: "KH", fav: {food: "beef"}}
```

`fav` 객체의 `food` 프로퍼티의 값을 `beef`로 변경하면 원본배열에도 영향을 주는 것을 확인할 수 있다. 이렇듯 객체를 프로퍼티로 가지는 객체에서 얕은 복사란 한 단계까지만 복사하는 것을 말한다.

객체 뿐 아니라 배열도 마찬가지이다.

```javascript
const origin = ['mon', { todo: 'reading' }];
const copy = [...origin];

copy[0] = 'thu';
copy[1].todo = 'cleaning';

console.log(origin); // ['mon', { todo: 'cleaning' }]
console.log(copy); // ['thu', { todo: 'cleaning' }]
```

스프레드 문법 대신 얕은 복사를 하는 대표적인 방법은 `Array.prototype.slice()` 메서드와 `Object.assign()` 메서드를 사용하는 것이다.

---

## 3. 깊은 복사(Deep copy)

깊은 복사는 데이터 자체를 완전히 복사하는 것으로 복사된 두 객체는 완전히 독립적인 메모리를 차지한다. 즉, 원본과의 참조가 완전히 끊어진다.

깊은 복사는 아래와 같이 세 가지의 경우로 나눌 수 있다.

1. 원시 값을 할당한 변수를 다른 변수에 할당하는 것
2. 객체를 프로퍼티 값으로 갖지 않는 객체의 경우 한 단계를 복사하는 것
3. 객체를 프로퍼티 값으로 갖는 객체의 경우 객체에 중첩되어 있는 객체까지 모두 복사하는 것

### 3-1. 원시 값을 할당한 변수를 다른 변수에 할당

```javascript
const origin = 'hello';
let copy = origin;

copy = 'world';

console.log(origin); // "hello"
console.log(copy); // "world"
```

원시 값은 기본 자료형으로 Number, String, Boolean, Null, Undefined 등이 해당된다. 이러한 원시 값은 할당한 변수를 다른 변수에 할당하게 되면 깊은 복사가 일어난다. 즉, 복사한 변수의 값을 바꾸면 원본 변수의 값은 변하지 않는다.

### 3-2. 객체를 프로퍼티 값으로 갖지 않는 객체의 경우 한 단계를 복사

아래와 같은 객체가 있다고 생각해보자.

```javascript
const obj = { name: 'HD', age: 30 };
const arr = [1, 2, 3, 4, 5];
```

위와 같은 객체는 대표적으로 스프레드 문법을 사용하여 깊은 복사를 할 수 있다.

```javascript
const objCopy = { ...obj };
const arrCopy = [...arr];

console.log(obj === objCopy); // false
console.log(arr === arrCopy); // false
```

깊은 복사를 하였기 때문에 원본 객체와 복사된 객체는 서로 다른 메모리를 가지고 있기 때문에 같지 않고 각각의 값(또는 요소)를 변경해도 서로 영향을 주지 않는다.

```javascript
objCopy.name = 'KH';
objCopy.age = 28;
arrCopy.push(6);

console.log(obj); // {name: 'HD', age: 30}
console.log(objCopy); // {name: 'KH', age: 28}

console.log(arr); // [1, 2, 3, 4, 5]
console.log(arrCopy); // [1, 2, 3, 4, 5, 6]
```

### 3-3. 객체를 프로퍼티 값으로 갖는 객체의 경우 객체에 중첩되어 있는 객체까지 모두 복사

이번에는 객체를 프로퍼티 값으로 갖는 객체의 경우를 알아보자.

```javascript
const origin = {
  name: 'HD',
  fav: {
    food: 'pizza',
  },
};
```

`origin` 객체는 `fav` 객체를 프로퍼티로 가지고 있다. 위 얕은 복사에는 `fav` 객체의 메모리를 함께 사용하고 있지만 깊은 복사에서는 `fav` 객체의 메모리 또한 원본과 복사 모두 독립적인 메모리를 가져야 한다.
어떻게 구현할 수 있을까? 간단하게 구현할 수 있는 방법은 두 가지가 있다.

1. JSON.parse, JSON.stringify
2. Lodash 라이브러리

이 뿐만 아니라 재귀 함수를 통한 구현도 있지만 이는 복잡하다는 것이 단점이다.

Lodash 라이브러리는 말 그대로 라이브러리를 그대로 가져와 사용할 수 있어 쉽고 안전하게 깊은 복사를 할 수 있지만 코딩 테스트에서는 사용할 수 없다는 것이 단점이다.

여기서는 JOSN.parse, JSON.stringify를 통한 깊은 복사를 해보자.

```javascript
const origin = {
  name: 'HD',
  fav: {
    food: 'pizza',
  },
};
const copy = JSON.parse(JSON.stringify(origin));

console.log(origin === copy); // false
console.log(origin.fav === copy.fav); // false
```

객체 내 `fav` 프로퍼티 까지 서로 다른 것을 볼 수 있다. 이는 각각 다른 메모리를 사용하고 있다는 것이다. 프로퍼티의 값을 변경해보자.

```javascript
copy.name = 'KH';
copy.fav.food = 'beef';

console.log(origin); // {name: "HD", fav: {food: "pizza"}}
console.log(copy); // {name: "KH", fav: {food: "beef"}}
```

복사된 객체의 프로퍼티를 변경해도 원본 객체에는 아무런 영향을 주지 않는 것을 확인할 수 있다.

---

## 4. Conclusion

> 얕은 복사와 깊은 복사를 정리해보았다. 지금까지 스프레드 문법을 사용하여 객체, 배열을 복사하여 사용했는데, 만약 프로퍼티로 객체가 있다면 해당 프로퍼티까지 깊은 복사가 되지 않았다는 것을 알게 되어서 좋은 공부가 된 것 같다. 또한 두루뭉실하게 알고 있었던 얕은 복사와 깊은 복사에 대해 알게되니 속이 시원한 느낌이 든다. 누군가가 이에 대해 질문을 하였으면 좋겠다. 잘 설명을 할 수 있을지는 모르겠지만 그런 과정에서 한 단계더 성장하고 개념을 내 것으로 만들 수 있지 않을까 싶다.

---

## 참고

[[JavaScript] 깊은 복사, 얕은 복사](https://bbangson.tistory.com/78)  
[깊은 복사 VS 얕은 복사](https://velog.io/@ellyheetov/Shallow-Copy-VS-Deep-Copy)

---

📅 2023-01-04
