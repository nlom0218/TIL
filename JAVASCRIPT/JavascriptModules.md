# 자바스크립트 모듈 시스템

## 1. 개요

모듈(파일)을 내보내고 불러오는 두 가지 방법에 대해 살펴보고 무엇이 선호되는지 알아보자.

---

## 2. 모듈의 의미

모듈이란? 애플리케이션을 구성하는 개별적 요소로 재사용 가능한 코드 조각이다.

이런 모듈은 기본적으로 비공개 상태이므로 다른 모듈에서 접근할 수 없다. 즉, 모듈은 개별적 존재로서 애플리케이션과 분리되어 존재한다.

그렇다면 어떻게 재사용을 할 수 있는 것일까? 다른 곳에서 재사용할 수 없으면 모듈은 존재의 의미가 없다. 때문에 아래와 같은 기능이 존재한다.

- export: 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개하는 것.
- import: 모듈이 공개한 자산 중 일부 혹은 전체를 선택해 자신의 스코프 내로 불러들여 재사용하는 것.

위와 같은 기능을 통해 재사용성이 좋아지고 개발 효율성과 유지보수성을 높일 수 있다.

## 3. 자바스크립트에서의 모듈

과거에는 다른 언어와 다르게 자바스크립트는 모듈이 성립하기 위해 필요한 파일 스코프와 import, export를 지원하지 않았다. `script` 태그를 사용하여 외부의 자바스크립트 파일을 로드할 수 있지만 파일마다 독립적인 파일 스코프를 갖지 않는다.

다시 말해, 여러 개의 자바스크립트 파일을 분리하여 `script` 태그로 로드해도 결국 하나의 자바스크립트 파일 내에 있는 것처럼 동작한다. 즉, 모든 자바스크립트 파일은 하나의 전역을 공유한다.

이런 자바스크립트에서도 모듈 시스템을 사용하기 위해 제안된 것이 CommonJS와 AMD다.

## 4. 모듈 시스템의 종류

1. AMD - 가장 오래된 모듈 시스템 중 하나로 require.js라는 라이브러리를 통해 처음 개발
2. CommonJS - Node.js 서버를 위해 만들어진 모듈 시스템
3. ESM - ES Module의 약자로 ES6(ES2015)에서 도입된 모듈 시스템

이중 `CommonJS`와 `ESM`을 비교해보자.

---

## 5. CommonJS

CommonJS 에서 모듈을 불러오고 내보내기 위해서는 `require / exports` 키워드를 사용한다.

### 5-1. 모듈 내보내기 - exports

```javascript
const calculator = {
  add(num1, num2) {
    return num1 + num2;
  },
};

module.exports = calculator;
```

`calculator` 객체를 만든 후 필요한 메서드를 작성하였다. 이후 `calculator` 객체를 `module.exports`을 함으로써 모듈화를 진행하였다. 이제 `calculator` 객체는 다른 파일에서도 사용가능하다.

### 5-2 모듈 가져오기 - require

```javascript
const calculator = require('./calculator');

const result = calculator.add(3, 4);
```

모듈을 가져오기 위해서는 위의 예시처럼 불러오고자 하는 모듈의 파일 경로를 `require` 에 작성하면 된다.

### 5-3. 모듈을 개별적으로 내보내고 가져오기

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

## 6. ESM

ES6에서 도입된 ESM 모듈 시스템에서 모듈을 가져오고 내보내기 위해서는 `import  / export` 키워드를 사용하면 된다.

### 6-1. import / export 사용하기

맨땅에 `import / export` 키워드를 사용하면 오류가 나타난다. 이를 해결하기 위해선 간단히 `package.json` 아래와 같은 키값을 추가하면 된다.

```javascript
{
    "type": "module"
}
```

### 6-2. 모듈 내보내기 - export

```javascript
const calculator = {
  add(num1, num2) {
    return num1 + num2;
  },
};

export default calculator;
```

리액트에서 사용하는 익숙한 모듈 내보내기이다. `export default` 를 사용하여 내보내고자 하는 모듈을 내보낼 수 있다.

### 6-3. 모듈 받아오기 - import

```javascript
import calculator from './calculator.js';

const result = calculator.add(3, 4);

console.log(result);
```

`import 모듈이름 from 모듈경로` 와 같이 작성하여 모듈을 불러올 수 있다.

### 6-4. 모듈 개별적으로 내보내고 받아오기

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

## 7. require / exports vs import / export

`require / exports` 키워드와 `import / export` 키워드는 동시에 사용할 수 없다. 그렇다면 어떤 방식이 더 선호될까? 일반적으로 `import / export` 키워드가 더 선호된다.

이유는 아래와 같다.

- 필요한 모듈 부분 만 선택하고 로드 할 수 있다.
- 성능이 우수하며 메모리를 절약한다.

---

## 8. Conclusion

> 리액트로 여러 프로젝트를 진행하다 보니 import, export에만 익숙해졌었는데 우테코 프리코스를 통해 require, exports을 사용하게 되었다. 하지만 현재 다시 import, export을 미션에 적용하고자 이 두개의 차이에 대해 알아보았다. 모듈(파일)을 단순히 받아온다는 공통점을 넘어 차이점도 분명 존재하고 성능이 뛰어나기 때문에 선호하는 모듈 시스템이 있으니 이를 잘 구별하자.

---

## 참고

모던 자바스크립트 Deep Dive - \_모듈  
[[javascript] module 시스템](https://doitnow-man.tistory.com/entry/javascript-module-%EC%8B%9C%EC%8A%A4%ED%85%9C)  
[[NODE] 📚 require vs import 문법 비교 (CommonJS vs ES6)](https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-require-%E2%9A%94%EF%B8%8F-import-CommonJs%EC%99%80-ES6-%EC%B0%A8%EC%9D%B5-1)

---

📅 2023-02-10  
📅 2023-03-29
