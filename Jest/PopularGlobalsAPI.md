# 자주 사용하는 Globals Methods

## 1. 개요

`describe`, `test`에 대해 정리한다.

---

## 2. describe

Test 단위를 묶는 가장 단위로 테스트시 `describe()` 함수에 설명된 내용으로 테스트 단위를 크게 분류한다. 테스트 코드 작성에 있어 `describe()` 함수는 필수사항은 아니다. 하지만 여러 테스트들을 하나의 그룹을 만들어 조직화하는 것에 도움을 준다.

---

### 2-1. describe(name, fn)

서로 연관된 테스트를 함께 묶는 그룹을 만드는 함수이다.

```javascript
const { describe, expect, test } = require("@jest/globals");
const calculation = require("../src/calculation");

describe("사칙 연산 테스트", () => {
  test("덧셈 테스트", () => {
    expect(calculation.sum(1, 2)).toBe(3);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(3, 1)).toBe(2);
  });
});
```

![describe(name, fn)](/image/Jest/PopularGlobalsAPI/describe1.png)

---

### 2-2. describe.each(table)(name, fn, tiemout)

`describe.each()` 함수를 사용하여 여러 테스트 데이터를 대상으로 테스트 함수를 실행할 수 있다. 이를 파라미터와 테스트라고 하며 이를 통해 더 깔끔하고 유지보수가 쉬운 테스트 코드를 작성할 수 있다.

- `table`: 중첩 배열, 각각의 행은 테스트에 전달된다.
- `name`: 문자열, 테스트 suites의 제목으로 함수를 실행하는 형식으로 작성한다. 매개변수 작성은 아래를 참고
  - `%s`: String
  - `%d`: Number
  - `%i`: Integer
  - `%f`: Floating point value
  - `%j`: JSON
  - `%o`: Object
  - `%#`: Index of the test case
- `fn`: 함수, `table`의 각 행의 매개변수를 함수 인수로 받는 함수이다.

```javascript
const { describe, expect, test } = require("@jest/globals");
const sum = require("../src/sum");

describe.each([
  [1, 2, 3],
  [2, 3, 5],
  [4, 5, 9],
])(".sum(%i, %i)", (a, b, expected) => {
  test(`${expected} 값을 반환한다.`, () => {
    expect(sum(a, b)).toBe(expected);
  });

  test(`${expected}보다 큰 값을 반환하지 않는다.`, () => {
    expect(sum(a, b)).not.toBeGreaterThan(expected);
  });

  test(`${expected}보다 큰 작은을 반환하지 않는다.`, () => {
    expect(sum(a, b)).not.toBeLessThan(expected);
  });
});
```

![describe.each(table)(name, fn, tiemout)](/image/Jest/PopularGlobalsAPI/describe2.png)

---

### 2-3. describe.only(name, fn)

특정 `describe block`만 테스트하기 위해 사용된다.

```javascript
const { describe, expect, test } = require("@jest/globals");
const calculation = require("../src/calculation");

describe("사칙 연산 테스트", () => {
  test("덧셈 테스트", () => {
    expect(calculation.sum(1, 2)).toBe(3);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(3, 1)).toBe(2);
  });
});

describe.only("백의 자리 사칙 연산 테스트", () => {
  test("덧셈 테스트", () => {
    expect(calculation.sum(100, 200)).toBe(300);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(300, 100)).toBe(200);
  });
});
```

![describe.only(name, fn)](/image/Jest/PopularGlobalsAPI/describe3.png)

주의할 점은 `$ npm test`로 모든 테스트를 진행할 경우 다른 파일의 테스트은 실행된다.

![describe.only(name, fn)](/image/Jest/PopularGlobalsAPI/describe4.png)

2개의 `Test Suites`와 11개의 `Tests`가 실행된 것을 확인할 수 있다.

---

### 2-4. describe.only.each(table)(name, fn)

`describe.each()` 함수에 `only`을 적용한 것이다. 테스트 실행 과정으 `describe.only(name, fn)`와 동일하다.

---

### 2-5. describe.skip(name, fn)

`only`와 반대 성격을 가진 `skip`은 건너뛰고 싶은 `describe`가 있을 때 사용한다.

```javascript
const { describe, expect, test } = require("@jest/globals");
const calculation = require("../src/calculation");

describe("사칙 연산 테스트", () => {
  test("덧셈 테스트", () => {
    expect(calculation.sum(1, 2)).toBe(3);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(3, 1)).toBe(2);
  });
});

describe.skip("백의 자리 사칙 연산 테스트", () => {
  test("덧셈 테스트", () => {
    expect(calculation.sum(100, 200)).toBe(300);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(300, 100)).toBe(200);
  });
});
```

![describe.skip(name, fn)](/image/Jest/PopularGlobalsAPI/describe5.png)

---

### 2-6. describe.skip.each(table)(name, fn)

`describe.each()` 함수에 `skip`을 적용한 것이다. 테스트 실행 과정으 `describe.skip(name, fn)`와 동일하다.

---

## 3. test

단일 테스트를 진행하기 위해 사용한다. `test`를 묶은 것이 바로 `describe`이다. `test`만 작성하여 테스트를 진행해도 괜찮다.

---

### 3-1. test(name, fn, timeout)

Jest를 사용하면서 가장 많이 사용되는 함수일 것이다.

```javascript
const { describe, expect, test } = require("@jest/globals");

test("join 메서드 테스트", () => {
  expect(["안", "녕"].join("")).toBe("안녕");
});
```

![test(name, fn, timeout)](/image/Jest/PopularGlobalsAPI/test1.png)

---

### 3-2. test.each(table)(name, fn, timeout)

`describe.each()` 함수와 같은 원리로 테스트를 진행한다.

- `table`: 중첩 배열, 각각의 행은 테스트에 전달된다.
- `name`: 문자열, 테스트 suites의 제목으로 함수를 실행하는 형식으로 작성한다. 매개변수 작성은 아래를 참고
  - `%s`: String
  - `%d`: Number
  - `%i`: Integer
  - `%f`: Floating point value
  - `%j`: JSON
  - `%o`: Object
  - `%#`: Index of the test case
- `fn`: 함수, `table`의 각 행의 매개변수를 함수 인수로 받는 함수이다.

```javascript
const { describe, expect, test } = require("@jest/globals");

test.each([
  [["강", "아", "지"], "강아지"],
  [["호", "랑", "이"], "호랑이"],
  [["고", "양", "이"], "고양이"],
  [["거", "북", "이"], "거북이"],
])(".join(%p) => %s", (arr, expected) => {
  expect(arr.join("")).toBe(expected);
});
```

![test.each(table)(name, fn, timeout)](/image/Jest/PopularGlobalsAPI/test2.png)

---

### 3-3. test.only(name, fn, timeout)

`test.only()` 함수는 특정 테스트만 실행하고 싶을 때 사용한다.

```javascript
const { describe, expect, test } = require("@jest/globals");
const calculation = require("../src/calculation");

describe("사칙 연산 테스트", () => {
  test.only("덧셈 테스트", () => {
    expect(calculation.sum(1, 2)).toBe(3);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(3, 1)).toBe(2);
  });
});
```

![test.only(name, fn, timeout)](/image/Jest/PopularGlobalsAPI/test3.png)

---

### 3-4. test.only.each(table)(name, fn)

`test.each()` 함수에 `only`을 적용한 것이다. 테스트 실행 과정으 `test.only(name, fn)`와 동일하다.

```javascript
const { describe, expect, test } = require("@jest/globals");

test.only.each([
  [["강", "아", "지"], "강아지"],
  [["호", "랑", "이"], "호랑이"],
  [["고", "양", "이"], "고양이"],
  [["거", "북", "이"], "거북이"],
])(".join(%p) => %s", (arr, expected) => {
  expect(arr.join("")).toBe(expected);
});

test("여러번 반복하기", () => {
  expect("안녕".repeat(2)).toBe("안녕안녕");
});
```

![test.only.each(table)(name, fn)](/image/Jest/PopularGlobalsAPI/test4.png)

---

### 3-5. test.skip(name, fn)

`test.skip()` 메서드는 특정 테스트를 스킵하고 싶을 때 사용하는 함수이다.

```javascript
const { describe, expect, test } = require("@jest/globals");
const calculation = require("../src/calculation");

describe("사칙 연산 테스트", () => {
  test.skip("덧셈 테스트", () => {
    expect(calculation.sum(1, 2)).toBe(3);
  });

  test("뺄셈 테스트", () => {
    expect(calculation.minus(3, 1)).toBe(2);
  });
});
```

![test.skip(name, fn)](/image/Jest/PopularGlobalsAPI/test5.png)

---

### 3-6 test.skip.each(table)(name, fn)

`test.each()` 함수에 `skip`을 적용한 것이다. 테스트 실행 과정으 `test.skip(name, fn)`와 동일하다.

```javascript
const { describe, expect, test } = require("@jest/globals");

test.skip.each([
  [["강", "아", "지"], "강아지"],
  [["호", "랑", "이"], "호랑이"],
  [["고", "양", "이"], "고양이"],
  [["거", "북", "이"], "거북이"],
])(".join(%p) => %s", (arr, expected) => {
  expect(arr.join("")).toBe(expected);
});

test("여러번 반복하기", () => {
  expect("안녕".repeat(2)).toBe("안녕안녕");
});
```

![test.skip.each(table)(name, fn)](/image/Jest/PopularGlobalsAPI/test6.png)

---

## 4. Conclusion

> Jest의 Globals API인 `discribe`과 `test`를 살펴보았다. 모든 내용을 다루진 않았지만 현재 내가 필요한 함수들은 정리한 듯 하다. 만약 다른 함수들이 필요하다면 그때 그때 내용을 추가하면서 학습하자. `expect` API도 바로 학습하여 테스트 코드를 본격적으로 작성하면서 프로그래밍 학습을 하도록 하자!  
> 저번주까지는 막연했던 Jest 테스트 코드가 이제는 약~~간 느낌이 오는 듯하다. 어떤 원리로 동작을 하는지, 어떤 메서드를 사용하여 다양한 테스트 코드를 작성할 수 있는지 말이다. 물론 아직은 본격적인 테스트 코드를 작성하지 않았고 완벽하게 다루지는 못하지만 꾸준히 하다보면 자연스럽게 체득되는 날이 온다고 믿는다.

---

## 참고

[[JEST] JEST의 기초](https://velog.io/@rlaghwns1995/JEST-JEST%EC%9D%98-%EA%B8%B0%EC%B4%88)  
[[Jest] 파라미터화 테스트: test.each(), describe.each()](https://www.daleseo.com/jest-each/)  
[Jest공식홈페이지 - Globals API](https://jestjs.io/docs/api#testname-fn-timeout)

---

📅 2022-11-06
