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
charCount("aaaa"); // { a: 4 }
charCount("hello"); // { h: 1, e: 1, l: 2, o: 1 }
```

### 3-2. 복잡한 예제

`Progress to More Complex Examples`

```javascript
charCount("hello my name is hd!"); // 문자가 아닌 기호, 공백 등은 어떻게 해야 할까? 또는 숫자가 있다면 포함해야 할까?
charCount("Hello hi"); // 대문자와 소문자를 어떻게 해야할까?
```

### 3-3. 빈 입력값이 있는 예제

`Explore Examples with Empty Inputs`

```javascript
charCount(""); // 빈 문자열인 경우 무엇을 반환해야 하는가?
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

1. Find the core difficulty in what you're trying to do
   - 어려운 것을 발견하게 되면
2. Temporarily ignore that difficulty
   - 그 부분을 무시하라
3. Write a simplified solution
   - 간단한 해결책을 작성하라
   - 보통 이 단계에서 어떻게 작동하는지 이해하게 된다.
4. Then incorporate that difficulty back in

예제: 문자열을 받고 각 문자의 수를 반환하는 함수를 작성하라.

예제 문제에서 반복문을 만드는 것을 어려워 한다면 문자열의 앞 몇 문자만 하드 코딩을 통해 해결책을 작성하라. 그렇게 된다면 반복문이 어떻게 동작하는지 이해할 수 있다.

혹은 공백, 문자/숫자가 아닌 다른 문자열, 대문자/소문자에 대해 다루는 것에 익숙하지 않다면 일단 그것들을 무시하고 코드를 작성하라. 그 후 구글링, 또는 면접관의 힌트를 통해 문제를 해결할 수 있도록 하는 것이 좋다.

즉, 어려운 것을 계속 붙잡고 있어봐야 시간만 소요될 뿐 정답에 근접할 순 없다. 그렇기 때문에 내가 당장 할 수 있는 것 부터 생각을 하자.

---

## 6. 5단계 - 되돌아 보기와 리팩터(Look Back and Refactor)

시간을 내어 코드를 살펴보고, 되돌아보고, 성찰하게 된다면 좋은 기회를 얻을 수 있다. 문제를 풀었다면 컴퓨터를 끄고 노는 것이 아니라 다시 한 번 코드를 살펴보는 습관을 가지도록 하자. 이러한 습관을 기르기 위해 아래의 질문을 스스로에게 던져보자.

- Can you check the result?
- Can you derive the result differently?
- Can you understand it at a glance? - 얼마나 직관적인가?
- Can you use the result or method for some other problem? - 직감을 발달시켜 다른 문제를 해결할 수 있는 직관력을 향상시켜 준다. 이전 다른 문제와의 유사점이 있는지?
- Can you improve the performance of your solution? - 성능을 향상시킬 수 있는 방법이 있는가?
- Can you think of other ways to refactor?
- How have other people solved this problem?

코드를 분석하고 성찰하고 되돌아보는 것이 그저 작동하도록 코드를 작성하고 하루를 마감하는 것보다 가치 있다.

---

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스

---

📅 2023-01-12
