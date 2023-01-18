# 객체의 프로퍼티 존재 확인과 열거

## 1. 개요

객체에 어떤 프로퍼티가 존재하는지 파악하는 방법과 프로퍼티를 열거하는 방법에 대해 알아본다. 해당 방법은 간혹 코딩 테스트에서 유용하게 사용될 수 있다.

---

## 2. 프로퍼티 존재 확인

### 2-1. in 연산자

`in 연산자`는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다. 기본 사용법은 아래와 같다.

```javascript
// key: 프로퍼티 키를 나타내는 문자열
// object: 객체포 평가되는 표현식
key in object;
```

예제를 통해 알아보자.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
};

console.log('name' in person); // true
console.log('fav' in person); // true
console.log('food' in person); // false
console.log('food' in person.fav); // true
```

사용법은 위와 같이 간단하다. 또한 `in 연산자`는 프로토타입 체인에 의하여 접근할 수 있는 속성에 대하여 `true`를 반환한다. 예를 들어 `toString` 프로퍼티는 `Object.prototype`의 메서드이므로 모든 객체에서 접근가능하다.

```javascript
console.log('toString' in {}); // true
```

### 2-2. Reflect.has 메서드

ES6에서 도입된 `Reflect.has` 메서드를 사용하여 프로퍼티의 존재를 확인할 수 있다. 이는 `in 연산자`와 동일하게 작동한다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
};

console.log(Reflect.has(person, 'name')); // true
console.log(Reflect.has(person, 'fav')); // true
console.log(Reflect.has({}, 'toString')); // true
```

### 2-3. Object.prototype.hasOwnProperty 메서드

`Object.prototype.hasOwnProperty` 메서드를 사용하여 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다. 단, 상속받은 프로토타입의 프로퍼티 키인 경우 `false`를 반환한다. 즉, 객체 고유의 프로퍼티 키인 경우에만 `true`를 반환한다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
};

console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('fav')); // true
console.log(person.hasOwnProperty('food')); // false
console.log(person.hasOwnProperty('toString')); // false
```

---

## 3. 프로퍼티 열거

### 3-1. for...in 문

객체의 모든 프로퍼티를 순회하여 열거(enumeration)하려면 `for...in 문`을 사용하면 된다. 더 정확히 말하자면 `for...in 문`은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트[[Enumerable]]의 값이 `true`인 프로퍼티를 순회하며 열거(enumeration)한다.

이를 통해 알 수 있는 것은 `toString`이 프로토타입 체인으로 인해 상속받은 프로퍼티이지만 열거 가능하지 않는 프로퍼티이기 때문에 `for...in 문`을 통해서 열거되지 않는다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
  __proto__: { age: 30 },
};

for (const key in person) {
  console.log(key); // name, fav, age
}
```

`Object.prototype.hasOwnProperty` 메서드와 함께 사용하여 자신의 고유한 프로퍼티만 열거할 수 있다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
  __proto__: { age: 30 },
};

for (const key in person) {
  if (!person.hasOwnProperty(key)) continue;
  console.log(key); // name, fav
}
```

`for...in 문`은 배열에서도 사용가능하다. 하지만 배열에는 이를 사용하지 말고 `for 문`이나 `for...of 문` 또는 `forEach` 메서드를 사용하기를 권장한다.

위에서 살펴본 바로는 `for...in 문`은 객체 자신의 고유 프로퍼티뿐 아니라 상속받은 프로퍼티도 열거한다. 때문에 자신의 고유 프로퍼티만 열거하기 위해선 `Object.prototype.hasOwnProperty` 메서드와 함께 사용해야 한다. 번거로운 작업이기 때문에 고유 프로퍼티만 열거하기 위해선 `Object.keys/values/entries` 메서드를 사용하는 것을 권장한다.

### 3-2. Object.keys 메서드

`Object.keys` 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
  __proto__: { age: 30 },
};

console.log(Object.keys(person)); // ['name', 'fav']
```

### 3-3. Object.values 메서드

ES8에서 도입된 `Object.values` 메서드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
  __proto__: { age: 30 },
};

console.log(Object.values(person)); // ["hd", {food: "pizza"}]
```

### 3-4. Object.entries() 메서드

ES8에서 도입된 `Object.entries` 메서드는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

```javascript
const person = {
  name: 'hd',
  fav: {
    food: 'pizza',
  },
  __proto__: { age: 30 },
};

console.log(Object.entries(person)); // [["name", "hd"], ["fav", {food: "pizza"}]]
```

---

## 4. Conclusion

> 배열을 다루는 것에만 많은 집중과 시간을 쏟았다. 때문에 일반 객체에 대한 학습이 부족하고 느껴 객체의 프로퍼티를 조회하고 열거하는 방법에 대해 정리를 하였다. 배열 또한 객체이기 때문에 오늘 정리한 방법을 사용할 수 있지만 배열 보다 일반 객체에 더욱 유용하게 사용될 것 같은 느낌이 든다. 코딩 테스트 문제를 풀다보면 일반 객체를 다룰 일이 몇몇 생기는데 그 때 마다 복잡하게 코드를 작성했던 경험이 있다. 쉽고 빠르고 가독성이 좋은 것이 있다면 얼른 배우고 습득하여 실전에 사용하도록 하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive  
[in 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/in)

---

📅 2023-01-18
