# 자바스크립트에서 함수를 선언하는 방법

## 1. 개요

자바스크립트에서 함수는 여러가지 방법으로 선언할 수 있다. 어떤 방법이 있는지 알아보고 각각의 특징과 차이점에 대해 정리한다.

---

## 2. 함수 선언식과 함수 표현식

### 2-1. 함수 선언 방법

1. 함수 선언식 - 일반적인 프로그래밍 언어에서의 함수 선언과 비슷한 형식

   ```javascript
   function foo() {}
   ```

2. 함수 표현식 - 유연한 자바스크립트 언어의 특징을 활용한 선언 방식

   ```javascript
   const foo = function () {};
   ```

---

### 2-2. 함수 선언식과 함수 표현식의 차이점

함수 선언식은 호이스팅에 영향을 받지만 함수 표현식은 영향을 받지 않는다.

> 호이스팅이란? 코드를 구현한 위치와 관계없이 브라우저가 자바스크립트를 해석할 때 맨 위로 끌어 올려지는 특징을 말한다.

즉, 함수 선언식으로 선언한 함수보다 함수 호출이 먼저 작성이 되어도 오류 없이 동작한다.

예시로 아래의 코드의 실행결과를 살펴보자.

```javascript
foo();
boo();

function foo() {
  console.log("hello");
}

const boo = function () {
  console.log("world");
};
```

![함수 호이스팅 실행 결과](/image/JS/JSFunction/functionResult.png)

위와 같은 결과가 나오는 이유는 호이스팅에 의해 자바스크립트 해석기는 아래와 같이 코드를 인식하기 때문이다.

```javascript
function foo() {
  console.log("hello");
}

let boo;

foo();
boo();

boo = function () {
  console.log("world");
};
```

---

## 3. 화살표 함수(arrow function)

화살표 함수는 함수 표현식보다 단순하고 간결한 문법으로 함수를 만들 수 있는 방법이다.

기본적인 사용방법은 아래와 같다.

```javascript
const foo = (arg1, arg2, ...arg3) => expression;
```

좀 더 구체적인 예시를 통해 함수 표현식과 화살표 함수를 비교해보자. 아래는 같은 함수를 함수 표현식으로 선언을 했느냐, 화살표 함수로 선언을 했느냐의 차이다.

```javascript
// 함수 표현식
const add = function (a, b) {
  return a + b;
};

// 화살표 함수
const add = (a, b) => a + b;

// 위 화살표 함수는 아래의 코드를 축약한 것
const add = (a, b) => {
  return a + b;
};
```

화살표 함수는 함수 본문이 한 줄이면 위의 예시처럼 간단하게 표현할 수 있다. 이런 측면에서 처음에는 가독성이 떨어지게 느껴지더라도 익숙해지면 훨씬 간결하고 가독성이 좋은 코드라는 것을 느낄 수 있다.

---

## 4. 함수 선언식과 화살표 함수의 차이

### 4-1. arguments

```javascript
// 일반 함수
function test1() {
  console.log(arguments);
}
test1(1, 2, 3, 4);

// 화살표 함수
const test2 = () => {
  console.log(arguments);
};
test2(1, 2, 3, 4);
```

![arguments](/image/JS/JSFunction/arguments.png)

- 일반 함수: 매개변수를 따로 받지 않아도 함수의 생성과 동시에 `arguments`를 통해서 값을 사용 가능
- 화살표 함수: `arguments` 객체가 바인딩 되지 않아 값을 사용하지 못함

---

### 4-2. 생성자 함수

```javascript
// 일반 함수
function Test1() {
  this.test = "test";
}
const test1 = new Test1();
console.log(test1.test);

// 화살표 함수
const Test2 = () => {
  this.test = "test";
};
const test2 = new Test2();
console.log(test2.test);
```

![constructor](/image/JS/JSFunction/constructor.png)

- 일반 함수: 생성자 함수로 사용할 때 자동으로 `constructor`를 호출
- 화살표 함수: `constructor`가 존재하지 않아 생성자 함수로 사용할 수 없음

---

### 4-3. method

```javascript
// 일반 함수
const obj1 = {
  name: "kim",
  hello: function () {
    console.log(`Hello, ${this.name}`);
  },
};
obj1.hello();

// 화살표 함수
const obj2 = {
  name: "kim",
  hello: () => {
    console.log(`Hello, ${this.name}`);
  },
};
obj2.hello();
```

![method](/image/JS/JSFunction/method.png)

- 일반 함수: 메소드로 사용되었을 때 `this`는 자기 자신을 호출한 객체를 대상으로 함
- 화살표 함수: 메소드 뿐 아니라 어디서 호출하더라도 `this`는 전역 객체를 가르킴, 전역 객체에는 `name` 값이 없기 때문에 `Hello,` 만 출력

---

### 4-4. prototype

```javascript
// 일반 함수
function test1() {}
console.log(test1.prototype);

// 화살표 함수
const test2 = () => {};
console.log(test2.prototype);
```

![prototype](/image/JS/JSFunction/prototype.png)

- 일반 함수: `prototype` 객체가 존재
- 화살표 함수: `prototype` 객체가 존재하지 않음

---

## 5. Conclusion

> 개인적으로 함수 선언식보다 화살표 함수를 선호하는 편이다. 더 간결해 보이고 더 최신에 등장한 개념이기 때문이다. 딱 여기까지만 생각하고 화살표 함수를 사용해 왔다. 그렇다고 함수 선언식의 방법을 몰랐던 것은 아니다. 내가 몰랐던 것은 그 두 방법으로 만들어진 함수의 차이였다. 앞으론 아무런 이유없이 화살표 함수만 사용하지 말고 이유를 가지고 사용하도록 하자. 즉, `4. 함수 선언식과 화살표 함수의 차이`에서 나타난 특징에 따라 함수 선언식으로 선언된 일반 함수가 사용해야 할 경우를 제외하곤 화살표 함수를 사용하도록 하자.  
> 또한 이번 파트를 공부하면서 앞으로 더 배우고 학습해야 할 내용이 많이 있다고 느꼈던 파트였다. 특히 항상 중요하다고 생각했던 자바스크립트의 동작 원리에 대해 다시 공부를 하도록 하자. 처음부터는 말고, 필요한 상황이 오면 뒤로 미루지 말고 부지런히 공부하자. 그리고 당장 필요한 학습은 `this`에 관한 내용이다.

---

## 참고

[함수 표현식 vs 함수 선언식](https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/)  
[화살표 함수 기본](https://ko.javascript.info/arrow-functions-basics)  
[함수선언식과 화살표 함수의 차이](https://code-reading.tistory.com/120)

---

📅 2022-10-30
