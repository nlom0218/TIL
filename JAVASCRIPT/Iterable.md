# 이터러블

## 1. 개요

ES6에 도입된 이터레이션 프로토콜에 대해 알아보며 이를 준수한 이터러블이 무엇인지 정리한다.

---

## 2. 이터레이션 프로토콜

ES6 이전에는 통일된 규약 없이 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등이 나름의 구조를 가지고 `for 문`, `for...in문`, `forEach 메서드` 등 다양한 방법으로 순회를 하였다.

이후 ES6dp 이터레이션 프로토콜이 도입되었고 이는 순회 가능한 데이터 컬렉션을 이터러블로 통일하여 `for...of 문`, `스프레드 문법`, `배열 디스트럭처링 할당`의 대상으로 사용할 수 있도록 일원화했다.

이터레이션 프로토콜에는 이터러블 프로토콜과 이터레이터 프로토콜이 있다. 이는 이터러블을 `for...of 문`, `전개 연사자` 등과 함께 동작하도록 한 규약이다.

간단한 예로 배열을 살펴보자. 검사도구에서 빈 배열을 만들고 해당 배열이 어떤 프로토타입이 있는지 확인해보자.

![iterable-array](/image/JS/Iterable/iterable-array.png)

`Symbol.iterator`를 프로토타입 체인으로 상속받은 것을 확인할 수 있다.

---

## 3. 이터러블

이터러블 프로토콜을 준수한 객체를 이터러블이라고 한다. 즉, 이터러블은 `Symbol.iterator`를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 만든다. 바로 위에서 확인한 배열도 바로 이터러블이다. 대포적인 이터러블에는 `배열`, `문자열`, `Map`, `Set`, `DOM 컬렉션`이 있다.

하지만 `일반 객체`는 이터러블이 아니기 때문에 이터러블만 가능한 `for...of 문`, `스프레드 문법`, `배열 디스트럭처링 할당`을 사용할 수 없다. 단, TC39 프로세스의 stage 4(Finished) 단계에 제안되어 있는 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용한다.

> `for...of 문`은 아래에서 다룬다. 하지만 `스프레드 문법`, `배열 디스트럭처링 할당`는 다른 챕터에서 자세히 다룰 예정이다.

---

## 4. 이터레이터

이터러블의 `Symbol.iterator` 메서드를 호추랗면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.

```javascript
const numbers = [1, 2, 3, 4, 5];
const numberIterator = numbers[Symbol.iterator]();
```

![iterable-iterator](/image/JS/Iterable/iterable-iterator.png)

이터레이터는 `next` 메서드를 가진다. `next` 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉, `next` 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 `iterator result object`를 반환한다.

```javascript
const numbers = [1, 2, 3, 4, 5];
const numberIterator = numbers[Symbol.iterator]();

let isDone = false;
while (isDone === false) {
  const result = numberIterator.next();
  console.log(result);
  isDone = result.done;
}
```

![iterable-iterator2](/image/JS/Iterable/iterable-iterator2.png)

`iterator result object`의 `value` 프로퍼티는 현재 순회 중인 이터러블의 값을 나타내며 `done` 프로퍼티는 이터러블의 순회 완료 여부를 나타낸다.

---

## 5. for...of 문

`for...of 문`은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다. `for...of문`은 내부적으로 이터레이터의 `next` 메서드를 호출하여 이러러블을 순회한다. `next` 메서드의 반환 값들은 아래와 같은 역할을 한다.

- `value` 프로퍼티: `for...of 문`의 변수에 할당
- `done` 프로퍼티: `false`이면 순회를 계획 `true`이면 순회를 중단

간단한 사용 예시이다. `for...in 문`의 형식과 유사하다.

```javascript
const string = 'Hello!';
for (let value of string) {
  console.log(value);
}
```

![iterable-for of](/image/JS/Iterable/iterable-forOf.png)

---

## 6. 이터러블과 유사 배열 객체

`NodeList`, `HTMLCollection`은 유사 배열 객체이면서 이터러블이다. 즉, ES6에 이터러블이 도입되면서 유사 배열 객체인 `NodeList`, `HTMLCollection` 객체에 `Symbol.iterator` 메서드를 구현하여 이터러블이 되었다.

하지만 모든 유사 배열 객체가 이터러블인 것은 아니다. 아래의 예제를 보자.

```javascript
const animals = {
  0: 'dog',
  1: 'cat',
  2: 'lion',
  length: 3,
};

for (let i = 0; i < animals.length; i++) {
  console.log(animals[i]); // dog, cat, lion
}
```

유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 가지고 있기 때문에 `for 문`으로 순회할 수 있다. 하지만 `for...of 문`으로 순회할 수 없다.

```javascript
for (let animal of animals) {
  console.log(animal);
}
```

![iterable-error](/image/JS/Iterable/iterable-error.png)

만약 `animals` 객체를 이터러블인 배열로 변환하고 싶으면 `Array.from` 메서드를 사용하면 된다. `Array.from` 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환해준다.

```javascript
const animalArray = Array.from(animals); // ['dog', 'cat', 'lion']
```

## 7. Conclusion

> 반복 혹은 순회 가능한 객체가 이터러블인 것을 여러 공부를 하면서 듣고 알고 있었다. 특히 문자열도 이터러블이기 때문에 `for...of 문`으로 순회하는 것을 코딩 테스트 문제를 풀며 익혔다. 자연스럽게 익힌 개념이었지만 이렇게 정리를 하니 머릿속에 더욱 잘 정리가 되는 느낌이다. 과연 다른 누군가에게 설명을 할 수 있을까? 기회가 된다면 시도해봐야겠다.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive  
[ES6에서의 순회와 이터러블:이터레이터 프로토콜](https://catsbi.oopy.io/79e5ee3a-7c93-4ed6-a48f-2da6eab42109)

---

📅 2023-01-17
