# 모의 함수(Mock Functions) 사용하기

## 1. 개요

Jest의 `Mock Functions`에 대해 정리한다.

---

## 2. Mock Functions(mockFn)이란?

Mock은 `거짓된`, `가짜의`의 뜻을 가진다. 즉, `mockFn`을 직역하자면 `가짜의 함수들`이 된다. 그리고 `Mocking`이라는 것은 단위 테스트를 작성할 때, 해당 코드가 의존하는 부분을 가짜(mock)로 대체하는 기법이다.

여기서 의문이 든다. 왜 `가짜의 함수들`을 만들어 테스트를 진행하는 것일까?

그 이유는, 테스트 하고 싶은 기능이 다른 기능들과 엮여있을 경우(의존) 정확한 테스트를 하기 힘들기 때문이다. 여기서 `mockFn`과 `Mocking`의 특징과 장점을 알 수 있다.

1. 실제 객체인 척하는 가짜 객체를 생성한다.
2. 테스트가 실행되는 동안 가짜 객체에 어떤 일들이 발생했는지 기억하기 때문에 가짜 객체가 내부적으로 어떻게 사용되는지 검증할 수 있다.
3. 실제 객체를 사용하는 것보다 훨씬 가볍고 빠르게 실행된다.
4. 항상 동일한 결과를 내는 테스트를 작성할 수 있다.

---

## 3. jest.fn()

`mockFn`를 통해 테스트를 하기 위해선 먼저 가짜 함수를 생성하는 과정을 거쳐야 한다. 이를 도와주는 것인 `jest.fn()` 함수이다.

`jest.fn()` 함수의 사용 방법은 아래와 같다.

```javascript
const mockFn = jest.fn();
```

생성된 가짜 함수는 일반 자바스크립트 함수와 동일한 방식으로 인자를 넘겨 호출할 수 있다.

```javascript
mockFn();
mockFn(1);
mockFn('nlom0218');
```

아직 아무런 메서드를 사용하지 않았기 때문에 위의 결과는 모두 `undefined`이다.

지금까지 `mockFn`을 생성하는 방법을 알아보았다. 매우 간단하게 만들 수 있다는 것을 알 수 있었다. 이제 생성된 `mockFn`에 사용할 수 있는 다양한 메서드를 알아보자.

---

### 3-1. mockFn.mockReturnValue(value)

`mockFn`가 리턴하는 값을 지정하는 메서드이다.

```javascript
const mock = jest.fn();

mock.mockReturnValue('1');
mock(); // 1

mock.mockReturnValue('Hello');
mock(); // 'Hello'

mock.mockReturnValue(true);
mock(); // true
```

---

### 3-2. mockFn.mockReturnValueOnce(value)

단 한번만 특정 값을 리턴하기 위해 사용되는 메서드이며 `메서드 체이닝`으로 연결될 수 있다. 더 이상의 `mockReturnValueOnce(value)` 메서드가 사용되지 않으면 `mockReturnValue(value)` 메서드로 지정된 값이 리턴된다.

```javascript
const mockFn = jest
  .fn()
  .mockReturnValueOnce(1)
  .mockReturnValueOnce(2)
  .mockReturnValueOnce(3)
  .mockReturnValue(0);

mockFn(); // 1
mockFn(); // 2
mockFn(); // 3
mockFn(); // 0
mockFn(); // 0
```

---

### 3-3. mockFn.mockImplementation(fn)

`mockFn`가 어떤 동작을 하는지 정해주는 메서드이다. 동작하는 `mockFn`를 만드는 것이라고 보면 된다.

```javascript
const mockFn = jest.fn((a, b) => a + b);

mockFn(1, 2); // 3

mockFn(10, 12); // 4
```

---

### 3-4. mockFn.mockImplementationOnce(fn)

`mockFn`의 동작을 한 번만 제한을 둘 때 사용되는 메서드이다. 단 한 번만 사용되며 이후엔 `mockImplementation(fn)` 메서드를 통해 정한 동작이 실행된다.

```javascript
const mockFn = jest.fn();

mockFn
  .mockImplementation((a, b) => a + b)
  .mockImplementationOnce((a, b) => a * b)
  .mockImplementationOnce((a, b) => a / b);

mockFn(3, 4); // 12 (3 * 4)
mockFn(18, 6); // 3 (18 / 6)
mockFn(2, 6); // 8 (2 + 6)
```

기본 동작은 `더하기`이지만, `곱하기`와 `나눗셈`이 순서대로 한 번씩 사용되는 코드이다.

---

### 3-5. mockFn.mockResolvedValue(value)

비동기 함수에서 `resolve` 값을 받을 때 사용된다. 추가적으로 `mockFn.mockResolvedValueOnce(value)` 메서드는 한 번 만 `resolve` 값을 받을 때 사용된다.

```javascript
test('async test', async () => {
  const asyncMock = jest.fn().mockResolvedValue(43);

  await asyncMock(); // 43
});
```

`mockResolvedValue(value)` 메서드는 `mockImplementation(fn)` 메서드로 표현이 가능하다. 비동기 함수를 `mockFn`로 만들 땐, `mockResolvedValue(value)` 메서드를 사용하는 것이 더욱 간결하고 가독성이 높다. 아래의 두 함수는 같은 역할을 한다.

```javascript
jest.fn().mockImplementation(() => Promise.resolve(value));

jest.fn().mockResolvedValue(value);
```

---

### 3-6. mockFn.mockRejectedValue(value)

비동기 함수에서 `reject` 값을 받을 때 사용된다. 한 번의 `reject` 값을 받기 위해선 `mockFn.mockRejectedValueOnce(value)` 메서드를 사용하면 된다.

```javascript
test('async test', async () => {
  const asyncMock = jest
    .fn()
    .mockRejectedValue(new Error('Async error message'));

  await asyncMock(); // throws 'Async error message'
});
```

`mockRejectedValue(value)` 메서드를 `mockResolvedValue(value)`로 나타낼 수 있다. 아래의 코드를 참고

```javascript
jest.fn().mockImplementation(() => Promise.reject(value));
```

---

## 4. Conclusion

> 실제 함수가 아니라 가짜 함수를 만들어 테스트를 진행하는 과정을 정리해보았다. 과연 이 가짜 함수가 얼마나 유용하게 사용이 되는지 직접 테스트를 작성해봐야 알겠지만, 일단은..! 유용한 기능이라는 것은 얼추 짐작이 된다.  
> 그렇다면 언제 가짜 함수를 만들어서 테스트 코드를 작성해야 하는 것인가? 데이터베이스를 다룰 때와 api를 다룰 때가 대표적이라고 생각한다. 실제로 데이터베이스를 다루는 것이 아니라 가짜로 데이터를 넣고 가짜로 결과를 받아오는 것이 실제 데이터베이스에 영향을 주지 않기 때문이다. api의 결과도 마찬가지이다.  
> 무작정 모든 함수를 가짜 함수로 만드는 것은 좋은 방법이 아닐 것 이다. 많은 테스트를 접해보면서 가짜 함수를 올바르게 사용하는 분별력을 기르도록 하자.

---

## 참고

[[JEST] 📚 모킹 Mocking 정리 - jest.fn / jest.mock /jest.spyOn](https://inpa.tistory.com/entry/JEST-%F0%9F%93%9A-%EB%AA%A8%ED%82%B9-mocking-jestfn-jestspyOn)  
[[Jest] jest.fn(), jest.spyOn() 함수 모킹](https://www.daleseo.com/jest-fn-spy-on/)

---

📅 2022-11-18
