# 자바스크립트 모듈 시스템

## 1. 개요

모듈(파일)을 내보내고 불러오는 두 가지 방법에 대해 살펴보고 무엇이 선호되는지 알아보자.

---

## 2. 모듈 시스템이란?

모듈은 하나의 파일이다. 이런 모듈을 다른 파일에서 불러와 사용할 수 있게 도와주는 것이 모듈 시스템이다. 즉, 어떤 함수를 모듈로 만들어 다른 파일에서 사용함으로써 함수의 재사용이 가능해지게 되는 것이다.

## 3. 모듈 시스템의 종류

1. AMD - 가장 오래된 모듈 시스템 중 하나로 require.js라는 라이브러리를 통해 처음 개발
2. CommonJS - Node.js 서버를 위해 만들어진 모듈 시스템
3. ESM - ES Module의 약자로 ES6(ES2015)에서 도입된 모듈 시스템

이중 `CommonJS`와 `ESM`을 비교해보자.

---

## 4. CommonJS

CommonJS 에서 모듈을 불러오고 내보내기 위해서는 `require / exports` 키워드를 사용한다.

### 4-1. 모듈 내보내기 - exports

```javascript
const calculator = {
  add(num1, num2) {
    return num1 + num2;
  },
};

module.exports = calculator;
```

`calculator` 객체를 만든 후 필요한 메서드를 작성하였다. 이후 `calculator` 객체를 `module.exports`을 함으로써 모듈화를 진행하였다. 이제 `calculator` 객체는 다른 파일에서도 사용가능하다.

### 4-2 모듈 가져오기 - require

```javascript
const calculator = require('./calculator');

const result = calculator.add(3, 4);
```

모듈을 가져오기 위해서는 위의 예시처럼 불러오고자 하는 모듈의 파일 경로를 `require` 에 작성하면 된다.

### 4-3. 모듈을 개별적으로 내보내고 가져오기

개별적으로 내보내기 위해선 `exports` 뒤에 바로 내보내고자 하는 모듈의 이름을 적은 후 해당 모듈이 무엇인지 정의하면 된다.

```javascript
exports.name = 'noah';
exports.age = 30;
```

이렇게 개별적으로 내보내진 모듈들은 아래와 같이 구조분해 할당으로 가져올 수 있다.

```javascript
const { name } = require('./noah');

console.log(name);
console.log(age);
```

---

## 5. ESM

ES6에서 도입된 ESM 모듈 시스템에서 모듈을 가져오고 내보내기 위해서는 `import  / export` 키워드를 사용하면 된다.

### 5-1. import / export 사용하기

맨땅에 `import / export` 키워드를 사용하면 오류가 나타난다. 이를 해결하기 위해선 간단히 `package.json` 아래와 같은 키값을 추가하면 된다.

```javascript
{
    "type": "module"
}
```

### 5-2. 모듈 내보내기 - export

```javascript
const calculator = {
  add(num1, num2) {
    return num1 + num2;
  },
};

export default calculator;
```

리액트에서 사용하는 익숙한 모듈 내보내기이다. `export default` 를 사용하여 내보내고자 하는 모듈을 내보낼 수 있다.

### 5-3. 모듈 받아오기 - import

```javascript
import calculator from './calculator.js';

const result = calculator.add(3, 4);

console.log(result);
```

`import 모듈이름 from 모듈경로` 와 같이 작성하여 모듈을 불러올 수 있다.

### 5-4. 모듈 개별적으로 내보내고 받아오기

```javascript
export const name = 'noah';
export const age = 30;
```

`const` 앞에 `export`만 붙으면 개별적으로 내보낼 수 있다!

```javascript
import { age, name } from './noah.js';

console.log(name);
console.log(age);
```

불러오는 것 또한 위 코드와 같이 간단하다.

---

### 6. require / exports vs import / export

`require / exports` 키워드와 `import / export` 키워드는 동시에 사용할 수 없다. 그렇다면 어떤 방식이 더 선호될까? 일반적으로 `import / export` 키워드가 더 선호된다.

이유는 아래와 같다.

- 필요한 모듈 부분 만 선택하고 로드 할 수 있다.
- 성능이 우수하며 메모리를 절약한다.

---

## 참고

[[javascript] module 시스템](https://doitnow-man.tistory.com/entry/javascript-module-%EC%8B%9C%EC%8A%A4%ED%85%9C)  
[[NODE] 📚 require vs import 문법 비교 (CommonJS vs ES6)](https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-require-%E2%9A%94%EF%B8%8F-import-CommonJs%EC%99%80-ES6-%EC%B0%A8%EC%9D%B4-1)

---

📅 2023-02-10
