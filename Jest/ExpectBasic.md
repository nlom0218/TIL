# 기본적인 Expect API 사용하기

## 1. 개요

이전까지의 챕터는 테스트를 하기 위한 빌드 과정이라고 할 수 있다. 실제 테스트를 위해 필요한 API는 `expect`라고 할 수 있다. 다양한 메서드들이 있는데 자주 사용되는 메서드 위주로 정리한다.

---

## 2. expect(value)

`expect()` 함수는 특정 값을 테스트하기 위해 항상 사용된다. `expect()` 함수는 `matcher` 함수와 함께 사용되는데 예제를 통해 이를 살펴보자.

예제를 살펴보기 전, `value`는 예상되는 값을 넣으면 된다. 즉 내가 테스트하고자 하는 반환값을 넣으면 된다.

```javascript
test('여러번 반복하기', () => {
  expect('안녕'.repeat(2)).toBe('안녕안녕');
});
```

위의 예제에서

1. `"안녕".repeat(2)`이 `value`이다. 즉 내가 테스트하고자 하는 대상이다.
2. `toBe()` 함수는 `matcher`에 해당한다.

또 다른 예제를 살펴보자. 아래의 예제에서 `bestFood()`의 리턴값은 `곱창`이다. 이를 테스트를 하는 코드이다.

```javascript
test('내가 제일 좋아하는 음식 테스트', () => {
  expect(bestFood()).toBe('곱창');
});
```

## 3. .toBe(value)

`.toBe(value)`는 결과값을 예측하기 위해 자주 사용된다.

```javascript
const HD = {
  name: 'hongdong',
  age: 29,
};

describe('HD 테스트', () => {
  test('이름 테스트', () => {
    expect(HD.name).toBe('hongdong');
  });

  test('나이 테스트', () => {
    expect(HD.age).toBe(29);
  });
});
```

![toBe](/image/Jest/ExpectBasic/toBe1.png)

`.toBe()` 메서드는 소수점이 있는 숫자와 함께 사용해서는 안된다. 자바스크립트에서 `0.2 + 0.1`은 `0.3`이 아니다. 이런 상황에서는 `toBeCloseTo()`를 사용해야 한다.

```javascript
describe('소수점 테스트', () => {
  test('toBe', () => {
    expect(0.1 + 0.2).toBe(0.3);
  });

  test('toBeCloseTo', () => {
    expect(0.1 + 0.2).toBeCloseTo(0.3);
  });
});
```

![toBe](/image/Jest/ExpectBasic/toBe2.png)

---

## 4. .toEqual(value)

`.toEqual(value)`는 `.toBe(value)`과 마찬가지로 결과값을 예측하기 위해 자주 사용된다. 하지만 이 둘은 엄연한 차이가 존재한다.

- `.toBe(value)`: 값과 객체가 모두 같은지 확인한다.
- `.toEqual(value)`: 값이 값은지 확인한다.

예를 들어 아래의 두 객체가 있다.

```javascript
const food1 = { name: 'pizza' };
const food2 = { name: 'pizza' };
```

`food1`과 `food2`의 값은 값다 하지만 서로 다른 객체이기 때문에 `.toBe(value)`로 비교하면 오류가 발생한다. 하지만 `.toEqual(value)`로 비교하면 테스트가 정상적으로 작동된다.

```javascript
const food1 = { name: 'pizza' };
const food2 = { name: 'pizza' };

describe('toBe와 toEqual 비교 테스트', () => {
  test('toBe', () => {
    expect(food1).toBe(food2);
  });

  test('toEqual', () => {
    expect(food1).toEqual(food2);
  });
});
```

![toEqual](/image/Jest/ExpectBasic/toEqual1.png)

---

## 5. .not

`.not`을 사용하며 반대의 테스트를 진행할 수 있다. 위의 `.toBe(value)` 테스트를 아래와 같이 바꾸어 보자.

```javascript
test('toBe', () => {
  expect(food1).not.toBe(food2);
});
```

![not](/image/Jest/ExpectBasic/not1.png)

오류없이 정상적으로 테스트가 통과되는 것을 확인할 수 있다.

---

## 6. 포함하는지 테스트하기

배열, 객체, 문자열을 테스트할 때 전체는 아니지만 부분이 포함되는지 테스트하기 위한 방법이다. 해당 방법은 `.toEqual()`와 같은 메서드의 인수로 전달된다. 바로 예제를 통해 살펴보자.

---

### 6-1. expect.arrayContaining(array)

```javascript
const arr = [1, 2, 3];

test('arrayContaining', () => {
  const expected = [1, 2];
  expect(arr).toEqual(expect.not.arrayContaining(expected));
});
```

![arrayContaining](/image/Jest/ExpectBasic/arrayContaining1.png)

---

### 6-2. expect.objectContaining(object)

```javascript
const { describe, expect, test } = require('@jest/globals');

const food = { name: 'pizza', price: '20000' };

test('objectContaining', () => {
  const expected = { name: 'pizza' };
  expect(food).toEqual(expect.objectContaining(expected));
});
```

![objectContaining](/image/Jest/ExpectBasic/objectContaining1.png)

---

### 6-3. expect.stringContaining(string)

```javascript
const name = 'kimhongdong';

test('stringContaining', () => {
  const expected = 'hong';
  expect(name).toEqual(expect.stringContaining(expected));
});
```

![stringContaining](/image/Jest/ExpectBasic/stringContaining1.png)

위 3개의 메서드는 모두 `.not`을 붙여 그 반대의 테스트를 진행할 수 있다.

ex) `expect.not.stringContaining(string)`

---

## 7. .toHaveLength(number)

`.toHaveLength(number)`는 배열, 문자열의 길이를 테스트하기 위해 사용한다. 주의해야 할 점은 숫자의 길이는 테스트를 할 수 없다. 숫자는 문자열로 바꾼 뒤 테스트를 진행하자.

```javascript
const string = 'hello';
const arr = [1, 2, 3];

test('toHaveLength', () => {
  expect(string).toHaveLength(5);
  expect(arr).toHaveLength(3);
});
```

![toHaveLength](/image/Jest/ExpectBasic/toHaveLength1.png)

숫자의 길이를 테스트하면 아래와 같이 오류가 나타난다.

```javascript
const number = 1234;

test('toHaveLength', () => {
  expect(number).toHaveLength(4);
});
```

![toHaveLength error](/image/Jest/ExpectBasic/toHaveLength2.png)

---

## 8. 그 외의 메서드

`Expect API`에는 위에서 살펴본 메서드 뿐 아니라 다양한 메서드가 있다. 그 중 몇가지를 추가적으로 간단히 살펴보며 마무리한다.

1. `.toBeTruthy()`: 참을 테스트한다.
2. `.toBeFalsy()`: 거짓을 테스트한다. `false`, `0`, `""`, `null`, `undefined`, `NaN`
3. `.toBeNull()`: `null`인지 테스트한다.
4. `.toBeUndefined()`: `undefined()`인지 테스트한다.
5. `.toBeNaN`: `NaN`인지 테스트한다.

---

## 9. Conclusion

> `Jest`에 대한 어느정도의 지식은 학습을 하였다. 이제 직접 프로젝트에 테스트 코드를 작성하여 실전에서 사용해보도록 하자. 물론 `Mock Functions`이 남아있긴 하지만 기초적인 테스트 코드는 충분히 작성할 수 있을 듯 하다. 실전에서 사용하면서 필요한 내용을 추가적으로 학습하여 정리하도록 하자.

---

## 참고

[Jest toBe(), toEqual() 차이](https://til.skylightqp.kr/f43419fa-53d5-42f4-90f4-32293618a5a6)  
[[JEST] JEST의 기초](https://velog.io/@rlaghwns1995/JEST-JEST%EC%9D%98-%EA%B8%B0%EC%B4%88)

---

📅 2022-11-07
