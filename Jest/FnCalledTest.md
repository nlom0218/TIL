# 함수의 호출 테스트하기 width jest.spyOn()

## 1. 개요

함수가 몇 번 호출되었는지, 어떤 인자를 받으며 호출되었는지 등에 대한 테스트 코드 작성 방법을 정리한다. 또한 아래의 등장하는 메서드들은 모두 `mockFn`에서만 사용이 가능하다. 즉, 진짜 함수에서는 사용하지 못한다. 하지만 `jest.spyOn()`을 이용하여 진짜 함수에서도 사용을 할 수 있다. 해당 방법도 정리한다.

---

## 2. .toHaveBeenCalled()

- 일반 함수가 아니라 `mockFn`에서만 사용가능 하다.

`mockFn`가 호출 되었는지 확인하는 메서드이다. 바로 코드를 작성해보자.

```javascript
test('test', () => {
  const add = (a, b) => a + b;

  add(1, 2);

  expect(add).toHaveBeenCalled();
});
```

위의 테스트 결과를 예상해보자. 성공했을까? 그렇지 않다. 위의 테스트는 실패한다. 그 이유는 `Matcher error` 메시지에서 찾을 수 있다.

> 참고로 `.toHaveBeenCalled()`와 같은 `expect`의 메서드가 `Matcher`이다.

![Matcher error](/image/Jest/FnCalledTest/MatcherError1.png)

`Matcher error`를 보면 `received value must be a mock or spy function`이라는 문구를 볼 수 있다. 즉, 이를 정리하면 아래와 같다.

`.toHaveBeenCalled()`는 `mockFn`에서만 사용이 가능하다.

그럼 `mockFn`을 만들어 테스트를 진행해보자.

```javascript
test('test', () => {
  const add = (a, b) => a + b;

  const mockFn = jest.fn(add);

  mockFn(1, 2);

  expect(mockFn).toHaveBeenCalled();
});
```

![toHaveBeenCalled](/image/Jest/FnCalledTest/toHaveBeenCalled.png)

테스트가 정상적으로 동작하는 것을 확인할 수 있다.

호출을 하지 않을 경우의 테스트는 `.not`을 작성하면 된다. 아래는 해당 경우의 예시이다.

```javascript
test('test', () => {
  const add = (a, b) => a + b;

  const mockFn = jest.fn(add);

  expect(mockFn).not.toHaveBeenCalled();
});
```

---

## 3. .toHaveBeenCalledTimes(number)

- 일반 함수가 아니라 `mockFn`에서만 사용가능 하다.

`mockFn`가 몇 번 호출되었는지 확인하는 메서드이다.

```javascript
test('test', () => {
  const add = (a, b) => a + b;

  const mockFn = jest.fn(add);

  mockFn(1, 2);
  mockFn(2, 3);

  expect(mockFn).toHaveBeenCalledTimes(2);
});
```

`Array.forEach()` 메서드를 통해 테스트 코드를 작성해보자.

```javascript
const helloEach = (fn, arr) => {
  arr.forEach((name) => fn(name));
};

test('test', () => {
  const hello = (name) => `Hello ${name}`;

  const mockFn = jest.fn(hello);

  helloEach(mockFn, ['HD', 'KH', 'GY']);

  expect(mockFn).toHaveBeenCalledTimes(3);
});
```

배열의 길이 만큰 `mockFn`이 호출되었다.

---

## 4. .toHaveBeenCalledWith(arg1, arg2, ...)

- 일반 함수가 아니라 `mockFn`에서만 사용가능 하다.

`mockFn`이 어떤 인자를 가지며 호출되었는지 확인하는 테스트이다.

```javascript
const helloEach = (fn, arr) => {
  arr.forEach((name) => fn(name));
};

test('test', () => {
  const hello = (name) => `Hello ${name}`;

  const mockFn = jest.fn(hello);

  helloEach(mockFn, ['HD', 'KH', 'GY']);

  expect(mockFn).toHaveBeenCalledWith('KH');
});
```

`.toHaveBeenCalledWith()` 메서드의 인자는 `mockFn`의 예상된 인자들 중(위의 예시에선 "HD", "KH", "GY") 중 하나만 만족하면 된다.

---

## 5. .toHaveBeenLastCalledWith(arg1, arg2, ...)

- 일반 함수가 아니라 `mockFn`에서만 사용가능 하다.

`mockFn`의 `마지막 호출`에서 어떤 인자를 받으며 호출되었는지 확인하는 메서드이다. 바로 위의 예시를 조금 변형하여 테스트 코드를 작성해보자.

```javascript
const introduceEach = (fn, arr) => {
  arr.forEach(({ name, age }) => fn(name, age));
};

test('test', () => {
  const introduce = (name, age) =>
    `My name is ${name} and i'm ${age} years old`;

  const mockFn = jest.fn(introduce);

  introduceEach(mockFn, [
    { name: 'HD', age: 29 },
    { name: 'KH', age: 27 },
    { name: 'GY', age: 30 },
  ]);

  expect(mockFn).toHaveBeenLastCalledWith('GY', 30);
});
```

`mockFn`은 차례대로 `name`과 `age`이라는 인자를 받는다. 그리고 순서상 `{ name: 'GY', age: 30 }`인자를 가진 `mockFn`이 가장 마지막에 호출이 된다.

---

## 6. .toHaveBeenNthCalledWith(nthCall, arg1, arg2, ....)

마지막에 호출된 `mockFn`의 인자를 확인하는 것이 아니라, 특정 순서에 호출된 `mockFn`의 인자를 확인하는 메서드이다.

```javascript
const introduceEach = (fn, arr) => {
  arr.forEach(({ name, age }) => fn(name, age));
};

test('test', () => {
  const introduce = (name, age) =>
    `My name is ${name} and i'm ${age} years old`;

  const mockFn = jest.fn(introduce);

  introduceEach(mockFn, [
    { name: 'HD', age: 29 },
    { name: 'KH', age: 27 },
    { name: 'GY', age: 30 },
  ]);

  expect(mockFn).toHaveBeenNthCalledWith(2, 'KH', 27);
});
```

---

## 7. jest.spyOn(object, methodName)

`mocking`에는 `spy`라는 개념이 있다. `spy`이는 말 그대로 스파이의 역할을 한다. 즉, 진짜 메서드의 정보를 몰래 빼내는 역할을 한다.

이를 이용하여 굳이 `mockFn`를 만들지 않고 메서드의 호출을 테스트할 수 있다.

예시를 통해 구체적인 사용을 알아보자.

```javascript
test('test', () => {
  const cal = {
    add: (a, b) => a + b,
  };

  const spyFn = jest.spyOn(cal, 'add');

  cal.add(1, 2);
  cal.add(2, 3);

  expect(spyFn).toHaveBeenCalledTimes(2);
  expect(spyFn).toHaveBeenLastCalledWith(2, 3);
});
```

어떤 메서드에 스파이를 달기 위해선 메서드는 객체에 포함되어 있어야 한다. 위의 예시에선 `cal`이라는 객체 내에 `add` 메서드가 있다. 이 `add` 메서드에 스파이를 달았다.

첫 번째 인자론 객체, 두 번째 인자론 문자열로 메서드 이름을 문자열로 넣으면 된다.

실제로 호출된 메서드는 `cal.add()`이고 이에 대한 정보를 몰래 캐고 있는 `spyFn`을 가지고 테스트를 진행하고 있다.

---

## 8. Conclusion

> 함수의 호출 테스트에 관련된 다양한 `.toHaveBeen**` 메서드에 대해 알아보았다. 또한 `jest.spyOn()` 함수도 알아보았는데 이는 실제 테스트 코드를 작성할 때 유용하게 사용될 듯 하다. Jest에는 정말 다양한 메서드들이 존재한다. 많은 것들을 한 번에 정리하고 적용할 수 없지만 지금처럼 조금씩 꾸준히 학습을 이어나가도록 하자. 물론 실전에 적용도 필수이다!

---

## 출처

[Jest공식홈페이지 - Expect](https://jestjs.io/docs/expect)  
[Jest공식홈페이지 - spyOn](https://jestjs.io/docs/jest-object#jestspyonobject-methodname)
