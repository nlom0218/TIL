# 정규 표현식의 메서드

## 0. 요약

| 메서드 | 설명                                                                      |
| ------ | ------------------------------------------------------------------------- |
| exec() | 문자열에서 일치하는 부분을 탐색한다. 배열또는 `null`을 반환한다.          |
| test() | 문자열에서 일치하는 부분이 있는 확인한다. `true` 또는 `false`를 반환한다. |

---

## 1. 개요

지금까지 정규 표현식이 무엇이며 플래그와 패턴에는 어떤 것들이 있는지 알아보았다. 이제 직접 메서드를 사용하여 정규 표현식 검사를 진행을 할 차례이다. 이때 필요한 정규 표현식의 메서드에 대해 살펴본다.

---

## 2. exec()

`exec()` 메서드는 문자열에서 일치하는 부분을 탐색하고 일치하는 부분이 존재하면 배열에 해당 정보를 담아서 반환한다. 만약 일치하는 부분이 없다면 `null`을 반환한다.

자바스크립트의 `RegExp` 객체는 `g` 또는 `y` 플래그를 설정한 경우 이전 일치의 인덱스를 저장하므로 상태를 가지고 있다. 때문에 문자열 내의 일치 다수를 반복해 순회할 수 있다.

### 2-1. exec() 메서드 사용예시와 반환값에 대한 설명

```javascript
const regExp = /m\w/g;
const string = 'Hello! my name is hd';
let result;

while ((result = regExp.exec(string)) !== null) {
  console.log(
    `일치한 전체 문자: ${result[0]}\n일치가 문자열에서 위치하는 인덱스: ${result['index']}\n원본 문자열: ${result['input']}\n다음 일치를 시작할 인덱스: ${regExp.lastIndex}`
  );
}
```

`string` 문자열에서 정규 표현식과 매칭되는 문자열은 `my`와 `me` 뿐이다. 때문에 반복문은 두 번 실행되고 이후 `result`에 `null`이 할당되어 반복문이 종료된다.

1. 첫번째 반복문의 결과
   - 일치한 전체 문자: my
   - 일치가 문자열에서 위치하는 인덱스: 7
   - 원본 문자열: Hello! my name is hd
   - 다음 일치를 시작할 인덱스: 9
2. 두번째 반복문의 결과
   - 일치한 전체 문자: me
   - 일치가 문자열에서 위치하는 인덱스: 12
   - 원본 문자열: Hello! my name is hd
   - 다음 일치를 시작할 인덱스: 14

매 반복문이 실행될 때 마다 `result` 변수에는 정규 표현식의 결과가 배열의 형태로 반환한다. 또한 정규 표현식 `regExp`의 `lastIndex` 속성 또한 계속 바뀐다. 단, `g` 플래그를 사용하지 않으면 항상 `0`이다. 위의 결과를 바탕으로 어떠한 값을 받아오는지 정리해보자.

- `result` 배열의 0번째 인덱스: 일치한 전체 문자
- `result` 배열의 `index` 속성: 일치가 문자열에서 위치하는 인덱스
- `result` 배열의 `input` 속성: 원본 문자열
- `regExp` 정규 표현식의 `lastIndex` 속성: 다음 일치를 시작할 인덱스(g를 누락하면 항상 `0`)

### 2-2. exec() 대체 메서드

문자열 내의 다수 일치를 수행할 수 있는 보다 간편한 신규 메서드가 제안된 상태이다. 바로 `String.prototype.matchAll()`이다. 해당 메서드는 다음 파트에서 자세하게 다룬다.

---

## 3. test()

`test()` 메서드는 주어진 문자열이 정규 표현식을 만족하는지 판별하고, 그 여부를 `true` 또는 `false` 반환한다. `test()` 메서드는 단순히 패턴이 문자열 내에 존재하는지에 대한 여부만 필요할 때 사용한다.

`exec()` 메서드와 마찬가지로 `test()` 메서드도 전역 탐색 플래그를 제공한 정규 표현식에서 여러 번 호출하면 이전 일치 이후부터 탐색을 한다. `exec()` 메서드와 `test()` 메서드를 혼용해 사용하는 경우에도 해당된다.

### 3-1. test() 메서드 사용예시1

```javascript
const regExp = /wo{2}/;
const string = 'woowoowoo';

regExp.test(string); // true
```

정규 표현식 `regExp`는 `woo`의 패턴을 찾는다. `string` 문자열에는 `woo`가 포한되어 있기 때문에 `test()` 메서드는 `true`를 반환한다.

이번엔 정규 표현식을 아래와 같이 바꾸어 보자.

```javascript
const regExp = /wo{3}/;
const string = 'woowoowoo';

regExp.test(string); // false
```

정규 표현식 `regExp`은 `wooo`의 패턴을 찾기 때문에 `string` 문자열에선 매칭 되는 것이 없다. 그 결과 `test()` 메서드는 `false`를 반환한다.

### 3-2. test() 메서드 사용예시2

이번엔 전역 플래그와 함께 `regExp`의 `lastIndex` 속성도 함께 살펴보자. `regExp`의 `lastIndex` 속성은 `exec()` 메서드에서 살펴본 것과 같이 다음 일치를 시작할 인덱스를 뜻한다.

```javascript
const regExp = /wo{2}/g;
const string = 'woowoowoo';
let result;

while ((result = regExp.test(string)) !== false) {
  console.log(
    `결과는 ${result}이고 다음으로 검색을 시작할 인덱스는 ${regExp.lastIndex}입니다.`
  );
}
```

위 코드에서 반복문은 총 3번 실행된다. 반복문이 실행되면서 콘솔에 찍히는 내용은 아래와 같다.

- 결과는 true이고 다음으로 검색을 시작할 인덱스는 3입니다.
- 결과는 true이고 다음으로 검색을 시작할 인덱스는 6입니다.
- 결과는 true이고 다음으로 검색을 시작할 인덱스는 9입니다.

마지막 결과에서 다음으로 검색을 시작할 인덱스는 `9`라고 한다. 하지만 `string` 문자열에서 `9` 이후에는 정규 표현식에 매칭되는 결과가 없기 때문에 `result`는 `false`를 반환하게 된다. 이렇게 반복문은 종료된다. 이렇게 사용하기 위해선 `g` 플래그를 함께 사용해야 한다.

### 3-3. test()와 비슷한 메서드

`RegExp.prototype.test()` 메서드와 비슷한 메서드는 바로 `String.prototype.search()` 메서드이다. 하지만, 차이점도 존재한다. `test()` 메서드는 단순히 결과를 불리언으로 반환하지만 `search()` 메서드는 정규 표현식과 일치한 경우 일치한 위치의 인덱스를 일치하지 않는 경우 `-1`를 반환한다.

`search()` 메서드에 관한 더욱 자세한 내용은 다음 파트에서 진행한다.

---

## 4. Conclusion

> RegExp 에서 사용할 수 있는 메서드를 정리하였다. 이를 정리하지 않고 사용할 땐, 정규 표현식의 메서드인지 문자열의 메서드인지 모른채 블로그의 내용을 그대로 옮겨 사용했다. 그래서 그런지 머리속에 개념이 정착되지 않아 항상 같은 내용을 찾아보곤 했다. 하지만 이렇게 정리를 하니 1차적으로 언제 사용할지에 대한 감이 잡힌다. `RegExp`의 `lastIndex` 속성도 유용하게 사용할 수 있을 것 같은 느낌이 든다.

---

## 참고

[MDN - 정규 표현식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions#javascript%EC%97%90%EC%84%9C_%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D_%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)  
[MDN - RegExp.prototype.exec()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)  
[MDN - RegExp.prototype.test()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)

---

📅 2023-01-27
