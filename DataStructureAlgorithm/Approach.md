# 문제 해결 접근법

## 1. 알고리즘이란 무엇인가?

알고리즘은 특정 작업을 달성하기 위한 과정이나 일련의 단계를 의미한다.

우리가 알고리즘을 알아야하는 이유는 우리들이 알고 있는 프로그래밍의 거의 모든 것이 특정 종류의 알고리즘을 포함하고 있기 때문이다. 또한 성공적인 문제 해결을 할 수 있는 개발자가 되기 위한 기반이 알고리즘이다. 그리고 취준생(나와 같은)인 경우 면접, 코딩 테스트에서도 중요하게 다루기 때문에 알고리즘은 중요하다고 할 수 있다.

---

## 2. 1단계 - 문제의 이해(Understand the Problem)

바로 코드를 작성하는 것 보다 한 걸음 물러서서 직면한 문제를 확실히 이해하는 것이 중요하다. 이를 위해 나에게 던질 수 있는 질문들의 목록을 바탕으로 문제를 이해하도록 하자. 또한 하나의 예제로 활용하여 질문에 대한 적절한 답변도 살펴보자.

예제: 두 숫자를 더하여 합계를 반환하는 함수를 작성하라.

### 2-1. 나만의 언어로 재구성할 수 있는가?

`Can I restate the problem in my own words?`

- 추가, 덧셈

### 2-2. 문제가 어떤 입력값을 담고 있는가?

`What are the inputs that go into the problem?`

- 숫자가 `int`인가? `float`인가?
- 큰 숫자를 표현하기 위한 문자열인가?

### 2-3. 어떤 출력값이 나와야 하는가?

`What are the outputs that should come from the solution to the problem?`

- `int`?, `float`?, `string`?

### 2-4. 문제를 해결할 충분한 정보가 주어졌는가?

`Can the outputs be determined from the inputs? In other words, do I have enough information to solve the problem?`

- 두 숫자가 아니라 하나의 숫자만 입력값으로 들어오게 되는 경우 어떤 값을 반환해야 하는가?

### 2-5. 문제에서 제공된 데이터 중 가장 중요한 부분은 어디인가?

`How should I label the improtant pieces of data that are a part of the problem?`

- 해당 문제에서 필요한 전부는 입력값과 출력값 두 가지이므로 인수로 전달받은 두 개의 숫자와 이를 합한 출력이 중요한 데이터이다.

예제는 간단하지만 복잡한 문제라면 중요한 데이터를 단계별로 생각하는 것이 중요하다.

---

## 3. 2단계 - 구체적인 예제들(Explore Concrete Examples)

예제를 생각하는 것은 문제를 잘 이해하는 데 도움이 된다. 또한 다양한 예제를 알고 있다면 구현한 작업을 확인할 수 있을 뿐 아니라 예제를 적용하면서 더 많은 정보를 습득할 수 있다.

구체적인 예제의 단계를 아래의 예제를 통해 알아보자.

예제: 문자열을 받고 각 문자의 수를 반환하는 함수를 작성하라.

### 3-1. 간단한 예제

`Start with Simple Examples`

```javascript
charCount('aaaa'); // { a: 4 }
charCount('hello'); // { h: 1, e: 1, l: 2, o: 1 }
```

### 3-2. 복잡한 예제

`Progress to More Complex Examples`

```javascript
charCount('hello my name is hd!'); // 문자가 아닌 기호, 공백 등은 어떻게 해야 할까? 또는 숫자가 있다면 포함해야 할까?
charCount('Hello hi'); // 대문자와 소문자를 어떻게 해야할까?
```

### 3-3. 빈 입력값이 있는 예제

`Explore Examples with Empty Inputs`

```javascript
charCount(''); // 빈 문자열인 경우 무엇을 반환해야 하는가?
```

### 3-4. 유효하지 않는 값이 있는 예제

`Explore Examples with Invalid Inputs`

```javascript
charCount(123); // 문자열이 아닌 숫자를 입력받은 경우
charCount(null); // 문자열이 아닌 다른 값을 입력받은 경우는 어떻게 해야 하는가?
```

보통 예외사항은 문제의 제한 사항에 명시되어 있으므로 꼼꼼하게 살펴봐야 한다.

---

## 4. 3단계 - 세부 분석(Break It Down)

`Explicitly write out the steps you need to take.`

문제를 해결하기 위한 단계들을 명확하게 작성을 해야 한다. 세세하고 모든 라인마다 작성하지 않아도 된다. 단순히 해결책의 기본적인 구성 요소만 작성하면 된다.

이는 실제로 코드를 입력하기 전 코드에 대해 한 번 생각해 볼 수 있도록 도와주고 이해되지 않는 부분들을 파악하게 하거나 이해할 수 있게 해준다.

예제: 문자열을 받고 각 문자의 수를 반환하는 함수를 작성하라.

```javascript
function charCount(string) {
  // make object to return at end
  // loop over string, for each character...
  // // if the char is number/letter AND a key in object, add one to count
  // // if the char is number/letter AND not in object, add it to object and set value to 1
  // // if char is something else (space, period, etc.) don't do anyting
  // retrun object at end
}
```

---

## 5. 4단계 - 해결 또는 단순화(Solve/Simplify)

---

## 6. 5단계 - 되돌아 보기와 리팩터(Look Back and Refactor)
