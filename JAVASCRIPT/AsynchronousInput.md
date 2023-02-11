# 자바스크립트에서 비동기 입력 다루기

## 1. 문제 상황

```javascript
function work() {
  setTimeout(() => {
    const start = Date.now();
    for (let i = 0; i < 1000000000; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
  }, 0);
}

console.log('작업 시작!');
work();
console.log('다음 작업');
```

문제 상황 코드를 보자. `work` 함수는 엄청난 반복문을 현재 실행하고 있다. 콘솔에 어떻게 출력이 될까?

아래와 같은 순서로 출력이 된다.

- 작업 시작!
- 다음 작업
- 470ms

하지만 `work` 함수의 작업을 마무리하고 다음 작업을 시작하고 싶다. 즉, 다음 작업이 `work` 함수의 동작이 모두 끝난 후 실행되고 싶은 것이다. 이를 위해 어떻게 해야할까?

위와 같은 상황을 해결하고 싶을 때, 자바스크립트에선 3가지 방법을 사용할 수 있다. 즉, 비동기 작업(입력)을 다루기 위해선 아래의 3가지 방법을 기억해두자.

- callback()
- Promise
- async/await

---

## 2. callback()

작업이 끝난 후 작업하고 싶은 코드를 콜백으로 넘겨주어 작업이 끝난 시점에 콜백을 실행시키면 된다. 아래의 코드를 살펴보자.

```javascript
function work() {
  setTimeout(() => {
    const start = Date.now();
    for (let i = 0; i < 1000000000; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
  }, 0);
}

console.log('작업 시작!');
work();
console.log('다음 작업');
```

아직 해결하기 전 코드이다. 여기에서 우리가 다음으로 작업할 것은 바로 `console.log("다음 작업")`이다. 이를 콜백함수로 넘기면 된다.

```javascript
function work(callback) {
  setTimeout(() => {
    const start = Date.now();
    for (let i = 0; i < 1000000000; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
    callback();
  }, 0);
}

console.log('작업 시작!');
work(() => {
  console.log('다음 작업');
});
```

이로써 `work` 함수의 기본 동작(반복문을 돌리고 걸린 시간을 콘솔로 찍는 것)이 끝난 후 `다음 작업`이 콘솔에 보이게 된다.

### 2-1. callback의 문제점

콜백 지옥이라고 들어봤는가? 바로 여러 동작을 계속해서 콜백으로 넘겨주게 되면 콜백지옥에 빠지게 된다. 예를 들어 아래와 같이 말이다.

```javascript
function work(num, callback) {
  setTimeout(() => {
    const start = Date.now();
    for (let i = 0; i < 100000000 * num; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
    callback();
  }, 0);
}

console.log('작업 시작!');
work(1, () => {
  work(2, () => {
    work(3, () => {
      work(4, () => {
        work(5, () => {
          work(6, () => {
            work(7, () => {
              work(8, () => {
                work(9, () => {
                  work(10, () => {
                    console.log('작업 끝');
                  });
                });
              });
            });
          });
        });
      });
    });
  });
});
```

들여쓰기만 봐도 토가 나올 지경이다... 그나마 `work` 함수 자체가 쉽고 콜백함수도 간단해서 다행이지 어떤 것이라도 복잡해지면 답이 없는 지경으로 갈 수 있다. 때문에 콜백함수만으로 비동기 입력 및 작업을 하기 보단 더 많은 방법을 사용하여 가독성이 좋은 코드를 만들 필요가 있다.

---

## 3. Promise

### 3-1. Promise 객체 만들기

`Promise` 객체를 만들기 위해선 아래와 같은 생성자 함수가 필요하다.

```javascript
new Promise(executor);
```

매개변수 `executor`은 `resolve` 및 `reject` 인수를 전달할 실행 함수이다. 실행 함수 내에서 성공적으로 작업을 마무리 할 경우 `resolve`에 데이터를 전달하고 실패할 경우 `reject`에 데이터를 전달한다. 간단한 `Promise` 객체를 만들어보자.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('끝!');
  }, 3000);
});

console.log(promise); // Promise { <pending> }

setTimeout(() => {
  console.log(promise); // Promise { '끝!' }
}, 3000);
```

위 `promise` 객체 내 `executor` 함수를 살펴보면 3초 뒤 `끝!`을 `resolve` 에 넘긴다. 그렇기 때문에 처음 `promise`에서는 아직 `pending`이, 3초 뒤엔 `끝!`이 콘솔에 찍히는 것을 볼 수 있다.

---

### 3-2. then(), catch()

`then()` 메서드는 두 개의 콜백함수를 인수로 받는다. 하나는 `Promise`가 성공했을 때, 다른 하나는 실패했을 때를 위한 콜백함수이다.

`catch()` 메서드는 하나의 콜백함수를 인수로 받는데, 이는 `Promise`가 실패했을 때를 위한 콜백함수이다.

> `then()`메서드에서 성공과 실패를 모두 다룰 수 있지만 실패는 보통 `catch()` 메서드에서 다루는 듯 하다.

간단한 예시를 살펴보자. 먼저 `then()` 메서드의 사용이다.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('끝!');
  }, 3000);
});

promise.then((result) => console.log(result)); // 3초 뒤 '끝!' 출력
```

`promise` 객체에서 3초 뒤에 반환되는 `resolve`의 데이터가 `then()` 메서드를 통해 받을 수 있다.

이번엔 `catch()` 메서드의 사용 예시이다.

```javascript
const promise = new Promise((resolve, reject) => {
  throw new Error('에러!');
});

promise.catch((error) => console.error(error)); // Error: 에러!
```

`promise` 객체에서 에러가 발생했기 때문에 `catch()` 메서들르 통해 해당 에러를 받을 수 있다.

### 3-3. 메서드 체이닝

`then()` 메서드와 `catch()` 메서드는 `Promise`를 반환하기 때문에 이를 연결하여 메서드 체이닝을 할 수 있다.

```javascript
const add = function (num1, num2) {
  return new Promise((resolve, reject) => {
    resolve(num1 + num2);
  });
};

add(1, 2)
  .then((result) => add(result, 3))
  .then((result) => add(result, 4))
  .then((result) => add(result, 5))
  .then((result) => add(result, 6))
  .then((result) => add(result, 7))
  .then((result) => add(result, 8))
  .then((result) => add(result, 9))
  .then((result) => add(result, 10))
  .then((result) => console.log(result));
```

이런 메서드 체이닝을 사용하여 콜백지옥을 벗어나 보자.

```javascript
const work = function (num) {
  return new Promise((resolve) => {
    const start = Date.now();
    for (let i = 0; i < 100000000 * num; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
    resolve(num + 1);
  });
};

work(1)
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result))
  .then((result) => work(result));
```

콜백지옥에서 벗어났다!

위의 코드 `then()` 메서드의 체이닝은 아래와 같이 리팩터링을 할 수 있다.

```javascript
new Array(9).fill(null).reduce((acc) => {
  return acc.then((result) => work(result));
}, work(1));
```

---

## 3. async/await

마지막으로 `async/await` 방법에 대해 살펴보자. `async/await` 문법은 `Promise` 를 더욱 쉽게 사용 할 수 있게 해준다. 아래는 각각의 키워드가 사용되는 때이다.

- async: 함수를 선언할 때 함수 앞 부분에 `async` 키워드를 붙인다.
- await: `Promise` 앞 부분엔 `await` 키워드를 붙인다.

정리하자면 `async/await` 문법을 사용하여 `Promise`의 결과를 기다린다는 것이다. 이런 `async/await` 문법은 한 쌍으로 하나만 단독으로 사용할 순 없다.

### 3-1. async

`async` 키워드가 붙여진 함수는 항상 프로미스를 반환한다.

```javascript
async function hello() {
  return 'hello';
}

hello().then((result) => console.log(result)); // hello
```

### 3-2. await

`await` 키워드를 만나게 되면 일단 자바스크립트는 프로미스가 모두 실행이 될 때 까지 기다린다.

```javascript
async function init() {
  const promise = new Promise((resolve) => {
    setTimeout(() => resolve(10), 2000);
  });

  const result = await promise;

  console.log(result); // 2초 뒤 '10' 출력
  console.log('정답입니다!'); // 위의 동작을 기다린 후 실행
}

init();
```

### 3-3. try/catch

`async/await` 문법에서 오류를 잡아내기 위해선 `try/catch` 구문을 사용한다.

```javascript
try {
} catch (error) {}
```

`try`에서는 비동기 작업에 대한 코드를 작성하고 `catch`에는 에러가 발생 했을 때의 코드를 작성하면 된다.

간단한 예시를 살펴보자.

```javascript
async function init() {
  try {
    await new Promise((resolve) => {
      setTimeout(() => resolve(10), 2000);
    });
    throw new Error('에러 발생!'); // 해당 지점에서 에러 발생
  } catch (error) {
    // 발생한 에러에 대한 작업
    console.error(error); // Error: 에러 발생!
  }
}

init();
```

---

## 4. Conclusion

> 자바스크립트에서 비동기 처리를 다루기 위한 방법을 정리해보았다. 내가 정리를 했지만 아직 부족한 부분이 많다고 느껴진다. 특히 3가지의 개념 모두 하나의 파트로 나누어 더 자세하게 정리해야 할 듯하다. 다른건 몰라도 특히 `async/awiat`은 더더욱! 우테코 미션을 진행하면서 입력값을 받는 것에 대한 비동기 처리를 고민하고 있는데 `Promise`와 `async/await`을 적용하여 친숙해지도록 해보자!

---

## 참고

[벨로퍼트와 함께하는 - 3장. 자바스크립트에서 비동기 처리 다루기](https://learnjs.vlpt.us/async/)
[벨로퍼트와 함께하는 - 01. Promise](https://learnjs.vlpt.us/async/01-promise.html)
[벨로퍼트와 함께하는 - 02. async/await](https://learnjs.vlpt.us/async/02-async-await.html)
[MDN - Promise.prototype.then()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)
[MDN - Promise.prototype.catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)  
[async와 await](https://ko.javascript.info/async-await)

---

📅 2023-02-11
