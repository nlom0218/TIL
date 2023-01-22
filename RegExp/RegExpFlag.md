# 정규식 플래그

## 1. 플래그란?

정규 표현식을 통해 특정 패턴을 가진 문자열을 검색할 때 고급 검색을 위해 전역 옵션을 지정해 줄 수 있는데 이를 플래그(flag)라고 한다.

자바스크립트는 6개의 플래그를 지원한다.

---

## 2. i(Ignore Case)

`i` 플래그는 대소문자 구분 없이 검색할 때 사용한다.

### 2-1. i 플래그 사용예시

먼저 i 플래그를 사용하지 않은 예시이다.

```javascript
const rexRep = /h/;
const string = 'Hello my name is hongdong';
```

![i flag](/image/RegExp/regexpFlag1.png)

문자열 중 `h`가 검색된 것을 볼 수 있다.

이번엔 i 플래그를 사용하여 검색해보자.

```javascript
const rexRep = /h/i;
const string = 'Hello my name is hongdong';
```

![i flag](/image/RegExp/regexpFlag2.png)

이번엔 `h`가 아니라 `h`보다 앞에 있는 `H`가 검색된 것을 볼 수 있다. 즉, i플래그는 `h`뿐 아니라 `H`도 검색을 가능하게 해준다.

---

## 3. g(Global)

`g` 플래그는 문자열 내 모든 패턴을 검색할 때 사용한다. 즉 검색 결과가 하나가 아니라 하나 이상일 수 있다. 반대로 `g` 플래그가 없으면 패턴과 일치하는 하나의 결과만 반환한다.

### 3-1. g 플래그 사용예시

먼저 g 플래그를 사용하지 않은 예시이다.

```javascript
const rexRep = /m/;
const string = 'Hello my name is hongdong';
```

![g flag](/image/RegExp/regexpFlag3.png)

가장 앞에 존재하는 `m` 문자만 검색되었다.

이번엔 g 플래그를 사용하여 검색해보자.

```javascript
const rexRep = /m/g;
const string = 'Hello my name is hongdong';
```

![g flag](/image/RegExp/regexpFlag4.png)

이번엔 문자열에 존재하는 모든 `m` 문자가 검색되었다.

### 3-2. i 플래그와 g 플래그 함께 사용하기

이번에는 i 플래그와 g 플래그를 함께 사용해보자.

```javascript
const rexRep = /h/gi;
const string = 'Hello my name is hongdong';
```

![g flag, i flag](/image/RegExp/regexpFlag5.png)

문자열에 존재하는 모든 `h` 또는 `H` 문자가 검색되었다.

---

## 4. m(Multi Line)

`m` 플래그는 문자열이 행이 바꾸더라도 검색을 계속 해주게 도와주는 역할을 한다. 해당 플래그는 나중에 다룰 `^` 앵커, `$` 앵커과 함께 사용된다.

예시에서는 `^` 패턴을 함께 사용할 것이다 `^` 앵커는 `문장열의 시작`을 의미한다.. 즉, 뒤에 나올 패턴으로 시작하는 문자열을 찾을 때 사용된다.

### 4-1. m 플래그 사용예시

m 플래그를 사용하지 않은 예시이다.

```javascript
const regExp = /^W/;
const string = 'Hello my name is hongdong\nWhat your name?';
```

![m flag](/image/RegExp/regexpFlag6.png)

`W` 문자로 시작하는 문자열을 찾으려 했지만 `What your name?`은 행이 바뀌었기 때문에 검색이 되지 않았다.

m 플래그를 사용해 보자.

```javascript
const regExp = /^W/m;
const string = 'Hello my name is hongdong\nWhat your name?';
```

![m flag](/image/RegExp/regexpFlag7.png)

m 플래그를 사용했기 때문에 행이 바뀌었더라도 `W` 문자로 시작하는 문자열을 찾을 수 있다.

### 4-2. m 플래그와 g 플래그 함께 사용하기

만약 여러 행이 있고 각 행에서 특정 패턴으로 시작하는 문자열을 검색하기 위해선 어떻게 해야 할까? 이럴땐 `m` 플래그와 함께 사용하면 된다.

```javascript
const regExp = /^W/m;
const string = 'Hello my name is hongdong\nWhat your name?\nUmm...\nWhat?';
```

![m flag, g flag](/image/RegExp/regexpFlag8.png)

두 번째 행 뿐 아니라 마지막 행의 `W`도 검색된 것을 볼 수 있다.

---

## 5. s

`s` 플래그는 `.`​(모든 문자 정규식)이 `개행 문자 \n`도 포함하도록 해준다.

즉, `.` 패턴에 개행 문자를 포함시키는 역할을 한다.

### 5-1. s 플래그 사용예시

먼저 사용하지 않은 예시이다.

```javascript
const regExp = /o.W/;
const string = 'Hello\nWhat your name?';
```

`o` 문자와 `W` 문자 사이에 어떤 문자 1개가 들어간 패턴을 찾는 정규식이다. 단, 개행 문자는 제외되기 때문에 반환되는 결과는 아래의 사진처럼 없다.

![s flag](/image/RegExp/regexpFlag9.png)

이번엔 s 플래그를 사용해보자.

```javascript
const regExp = /o.W/s;
const string = 'Hello\nWhat your name?';
```

하지만 s 플래그를 사용한다면 결과는 달라진다. s 플래그가 `.`에 개행 문자도 포함시켰기 때문이다.

![s flag](/image/RegExp/regexpFlag10.png)

---

## 6. u(unicode)

`u` 플래그는 유니코드까지 검색을 하도록 도와준다.

### 6-1. u 플래그 사용예시

통화 단위를 가지고 살펴보자. 통화 단위를 정규식을 검사하기 위해서는 유니코드 프로퍼티 `\p{Currency_Symbol}`를 사용해야 한다. 짧게 `\p{Sc}`로 사용해도 된다.

```javascript
const regExp = /\p{Sc}/gu;
const string = "Prices: $2, €1, ¥9
```

u 플래그 뿐 아니라 g 플래그도 사용하여 모든 통화 단위를 검사하고자 한다. 결과는 아래의 사진과 같다. 이때 u 플래그가 없다면 어떤 통화 단위도 검사를 통과하지 못한다.

![u flag](/image/RegExp/regexpFlag11.png)

---

## 7. y(sticky)

`y` 플래그는 문자 내 특정 위치에서 검색을 진행하는 ‘sticky’ 모드를 활성하는 역할을 한다.

`y` 플래그를 이해하기 위해서는 정규식의 `lastIndex`에 대한 개념을 알고 있어야 한다.

### 7-1. 정규식의 lastIndex

아래와 같은 코드를 호출했을 때 정규식의 lastIndex의 값을 보면서 과연 정규식의 lastIndex는 무엇을 의미하는지 알아보자. `\w+` 패턴은 이후에 다룰 예정이니 여기서는 `lastIndex`의 파악에 힘을 써보자.

```javascript
const string = 'Hello, my name is hongdong';
const regExp = /\w+/g;

regExp.exec(string); // ['Hello', index: 0, input: 'Hello, my name is hongdong', groups: undefined]
regExp.lastIndex; // 5

regExp.exec(string); // ['my', index: 7, input: 'Hello, my name is hongdong', groups: undefined]
regExp.lastIndex; // 9

regExp.exec(string); // ['name', index: 10, input: 'Hello, my name is hongdong', groups: undefined]
regExp.lastIndex; // 14

regExp.exec(string); // ['is', index: 15, input: 'Hello, my name is hongdong', groups: undefined]
regExp.lastIndex; // 17

regExp.exec(string); // ['hongdong', index: 18, input: 'Hello, my name is hongdong', groups: undefined]
regExp.lastIndex; // 26

regExp.exec(string); // null
regExp.lastIndex; // 0
```

위의 실행 결과를 살펴보자. `exec()` 메서드가 호출되면 정규식에 일치하는 결과를 차례대로 호출한다. 첫 번째로 일치하는 결과는 `Hello`이고 해당 문자는 문자열에서 `0`번 인덱스부터 `4`번 인덱스에 해당된다. 그 후 `lastIndex`를 호출하면 다음으로 검사를 진행하게 될 인덱스를 반환한다.

- 첫 번째 호출: 0번 인덱스에서 시작, 4번 인덱스에서 끝 -> 다음은 5번 인덱스에서 시작
- 두 번째 호출: 7번 인덱스에서 시작, 8번 인덱스에서 끝 -> 다음은 9번 인덱스에서 시작
- 세 번째 호출: 10번 인덱스에서 시작, 13번 인덱스에서 끝 -> 다음은 14번 인덱스에서 시작
- 네 번째 호출: 15번 인덱스에서 시작, 16번 인덱스에서 끝 -> 다음은 17번 인덱스에서 시작
- 다섯 번째 호출: 18번 인덱스에서 시작, 25번 인덱스에서 끝 -> 다음은 26번 인덱스에서 시작
- 여섯 번째 호출: 더 이상 일치하는 문자가 없기 때문에 null를 반환 -> 검사가 끝났으므로 lastIndex는 0

위와 같은 과정이 나타났음을 알 수 있다. 참고로 `test()` 메서드도 이와 같이 동작한다.

---

### 7-2. y 플래그 사용예시

```javascript
const string = 'Hello, my name is hongdong';
const regExp = /\w+/y;

regExp.lastIndex = 5;
regExp.exec(string); // null
```

`string`의 5번째 인덱스는 `,`이다. 즉, 문자가 아니기 때문에 `null`를 반환한다.

```javascript
regExp.lastIndex = 12;
regExp.exec(string); // ['me', index: 12, input: 'Hello, my name is hongdong', groups: undefined]
```

`string`의 12번째 인덱스는 `m`이다. 즉, 문자이기 때문에 `m`부터 이어지는 문자를 반환한다.

```javascript
regExp.lastIndex = 18;
regExp.exec(string); // ['hongdong', index: 18, input: 'Hello, my name is hongdong', groups: undefined]
```

`string`의 18번째 인덱스는 `h`이다. 즉, 문자이기 때문에 `h`부터 이어지는 문자를 반환한다.

---

## 8. Conclusion

> 플래그에 대해 살펴보았다. 예전에는 중구난방으로 무엇을 뜻하는지 모른채 사용을 했지만 정리를 하고 나니 언제 사용해야 할지 감이 조금씩 잡힌다. `y` 플래그는 이해하기가 너무 어려웠다.ㅠㅠ 부족한 내용, 틀린 내용이 있다면 이후에 수정하여 채워나가자.

---

## 참고

[패턴과 플래그](https://ko.javascript.info/regexp-introduction)  
[[JS] 📚 정규표현식(RegExp) - 이해하기 쉽게 정리 + 응용 예제](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)  
[유니코드: 'u' 플래그와 \p{...} 클래스](https://ko.javascript.info/regexp-unicode)  
[Sticky flag "y", searching at position](https://ko.javascript.info/regexp-sticky)

---

📅 2023-01-22
