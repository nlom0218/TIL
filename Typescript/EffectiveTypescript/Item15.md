# 아이템 15 - 동적 데이터에 인덱스 시그니처 사용하기

## 1. 인덱스 시그니처란?

공식문서에는 아래와 같이 인덱스 시그니처의 사용 시점에 대해 설명하고 있다.

> Sometimes you don’t know all the names of a type’s properties ahead of time, but you do know the shape of the values.  
> In those cases you can use an index signature to describe the types of possible values.

타입의 속성을 모르지만 값의 형태를 알고 있을 때, 인덱스 시그니처를 사용하여 타입을 설명할 수 있다.

---

## 2. 인덱스 시그니처의 기본 문법

```javascript
type Props = { [property: string]: string };
```

`Props` 타입이 있다. 여기에서 인덱스 시그니처는 `[property: string]: string`에 해당합니다. 이러한 인덱스 시그니처는 댜음 세 가지 의미를 담고 있다.

1. 키의 이름
   - 위 예제에서의 `property`에 해당
   - 키의 위치만 표시하는 용도
   - 무시할 수 있는 참고 정보 - 반드시 `property`가 아니여도 된다. 다른 단어도 상관없다.
2. 키의 타입
   - 위 예제에서의 첫번째 `string`에 해당
   - `string`, `number`, `symbol` 중 하나
   - 보통은 `string`을 사용
3. 값의 타입
   - 위 예제에서의 세번째 `string`에 해당
   - 모든 타입이 가능

---

## 3. 인덱스 시그니처의 단점

다음 예제를 통해 인덱스 시그니처의 단점에 대해 살펴보자.

```javascript
type Person = { [key: string]: string };
const person: Person = {
  name: 'noah',
  address: 'somewhere',
  age: '30',
};
```

### 3-1. 잘못된 키를 포함해 모든 키를 허용한다.

`key`는 `string`이기만 하면 어떠한 값도 올 수 있다. 때문에 `name`이 아니라 `Name`으로 키의 이름을 정하여도 오류가 나지 않는다.

```javascript
const person: Person = {
  Name: 'noah',
  address: 'somewhere',
  age: '30',
};
```

### 3-2. 특정 키가 필요하지 않다.

`person` 객체에 반드시 존재해야 할 속성(키-값)을 정할 수 없다. 때문에 `{}`도 허용한다.

```javascript
const person: Person = {};
```

### 3-3. 키마다 다른 타입을 가질 수 없다.

위의 예시에서 `age`는 `string`이 아니라 `number`가 더 적합하다.

하지만 `age`를 `number`로 하면 타입스크립트는 오류를 나타낸다.

### 3-4. 자동 완성 기능이 동작하지 않는다.

어떠한 속성이 있는지 알 수 없기 때문에 객체를 만들 때, 자동완성 기능을 사용할 수 없다.

또한, 객체를 만들었더라도 객체의 속성에 접근하는 과정에서도 자동완성 기능을 사용할 수 없다.

다음은 타입스크립트를 사용함으로써 얻을 수 있는 자동완성 기능의 예시다. 즉, 인덱스 시그니처를 사용하면 아래와 같은 기능을 사용할 수 없다.

![자동완성](/image/Typescript/EffectiveTypescript/item15-1.png)
![자동완성](/image/Typescript/EffectiveTypescript/item15-2.png)

---

## 4. 그렇다면 인덱스 시그니처는 언제?

가능하다면 인덱스 시그니처보다 정확한 타입을 사용하는 것이 좋다. 하지만 특별한 경우엔 인덱스 시그니처를 사용해야 한다. 바로 런타임 때까지 객체의 속성을 알 수 없을 경우이다.

정확한 속성의 이름을 알지 못하기 때문에 미리 타입으로 정할 수 없기 때문이다.

### 4-1. CSV 파일을 객체로 바꾸기

아래의 표를 보자.

| 연도 | 제조사 | 모델                     | 설명                            | 가격    |
| ---- | ------ | ------------------------ | ------------------------------- | ------- |
| 1997 | Ford   | E350                     | ac abs moon                     | 3000.00 |
| 1999 | Chevy  | Venture Extended Edition |                                 | 4900.00 |
| 1999 | Chevy  | Venture Extended Edition | Very Large                      | 5000.00 |
| 1996 | Jeep   | Grand Cherokee           | MUST SELL! air moon roof loaded | 4799.00 |

첫 번째 열은 header이며 이는 어떤 데이터를 가져와 표현할 것인지에 따라 달라질 수 있다. 즉, header의 내용이 더 많아질 수도, 적어질 수도 혹은 다른 이름으로 바뀔 수 있다. 이런 경우 우린 header가 무엇인지 확정지을 수 없는 문제가 생긴다. 이런 데이터를 `동적 데이터`라고 하며 `동적 데이터`를 다루는 경우 인덱스 시그니처를 통해 타입을 정할 수 있다.

위와 같은 표를 CSV 파일 형식으로 바꾸면 아래와 같다.

> CSV(영어: comma-separated values)는 몇 가지 필드를 쉼표(,)로 구분한 텍스트 데이터 및 텍스트 파일이다

```javascript
const exampleCSV = `연도,제조사,모델,설명,가격
1997,Ford,E350,ac abs moon,3000.00
1999,Chevy,Venture Extended Edition,,4900.00
1999,Chevy,Venture Extended Edition,Very Large,5000.00
1996,Jeep,Grand Cherokee,MUST SELL! air moon roof loaded,4799.00`;
```

각 열의 값을 header의 이름과 매핑하는 객체로 나타내어 보자.

```javascript
function parseCSV(input: string): { [columnName: string]: string }[] {
  const lines = input.split('\n'); // 각 열을 나눈다.
  const [header, ...rows] = lines; // 첫번째 열은 header, 나머지는 rows로 나눈다.

  const headerColumns = header.split(','); // header의 내용을 ,를 기준으로 나눈다.
  // 위 과정을 통해 나온 값들이 정확히 어떤 값인지 런타임 이전에 모르기 때문에 인덱스 시그니처를 사용한다.

  return rows.map((rowStr) => {
    const row: { [columnName: string]: string } = {}; // 리턴 할 값
    rowStr.split(',').forEach((cell, i) => {
      row[headerColumns[i]] = cell; // 각 행에 맞는 header의 이름을 매팽하여 row에 추가한다.
    });

    return row; // 반환
  });
}
```

다음은 결과값이다.

```javascript
parseCSV(exampleCSV);
[
  {
    연도: '1997',
    제조사: 'Ford',
    모델: 'E350',
    설명: 'ac abs moon',
    가격: '3000.00',
  },
  {
    연도: '1999',
    제조사: 'Chevy',
    모델: 'Venture Extended Edition',
    설명: '',
    가격: '4900.00',
  },
  {
    연도: '1999',
    제조사: 'Chevy',
    모델: 'Venture Extended Edition',
    설명: 'Very Large',
    가격: '5000.00',
  },
  {
    연도: '1996',
    제조사: 'Jeep',
    모델: 'Grand Cherokee',
    설명: 'MUST SELL! air moon roof loaded',
    가격: '4799.00',
  },
];
```

### 4-2. 열(header)이름을 알고 있는 경우

하지만 열 이름을 알고 있는 특정한 상황이라면 미리 선언해 둔 타입으로 단언문을 사용하는 것이 좋다.

```javascript
type Product = {
  연도: string;
  제조사: string;
  모델: string;
  설명: string;
  가격: string;
};

const products = parseCSV(exampleCSV) as unknown as Product;
```

---

## 5. 연관 배열에서의 인덱스 시그니처

연관 배열이란? 키 하나와 값 하나가 연관되어 있으며 키를 통해 연관되는 값을 얻을 수 있는 자료구이다.

예를들어 문자열 내의 단어 개수를 구해 객체로 만든다면 키는 문자가 될 것이고 값은 키가 문자열에서 등장한 수가 된다.

"Objects have a constructor"이라는 문자열을 가지고 위 설명을 객체를 만드면 예상되는 값은 다음과 같다.

```javascript
{
  Objects: 1;
  have: 1;
  a: 1;
  constructor: 1;
}
```

이와 같은 객체를 생성하는 함수를 인덱스 시그니처와 Map 타입을 이용하여 만들어보자.

### 5-1. 인덱스 시그니처를 사용한 함수

```javascript
function countWords(text: string) {
  const counts: { [word: string]: number } = {};

  const words = text.split(/[\s,.]+/);

  words.forEach((word) => {
    counts[word] = ([word] || 0) + 1;
  });

  return counts;
}
```

위 함수를 통해 결과를 살펴보면 다음과 같다.

```javascript
{
  Objects: 1,
  have: 1,
  a: 1,
  constructor: 'function Object() { [native code] }1'
}
```

`constructor` 부분이 이상하다. 이는 `constructor`의 초기값이 `undefined`가 아니라 `Object.prototype`에 있는 생성자 함수이기 때문이다.

### 5-2. Map 타입을 사용한 함수

```javascript
function countWords(text: string) {
  const counts: Map<string, number> = new Map(); // Map 타입을 사용

  const words = text.split(/[\s,.]+/);

  words.forEach((word) => {
    counts.set(word, (counts.get(word) || 0) + 1);
  });

  return counts;
}
```

```javascript
Map(4) { 'Objects' => 1, 'have' => 1, 'a' => 1, 'constructor' => 1 }
```

원하는 결과를 볼 수 있다.

---

## 6. 타입을 조금 더 좁히기

다음의 상황을 가정해보자.

- 데이터에 a, b, c, d, e의 키가 있다.
- a, b, c, d, e는 모두 필수가 아니다.
- 데이터에 정확히 얼마만큼 키가 들어있는지 모른다.

위와 같은 상황에서 인덱스 시그니처를 사용할 수 있다.

```javascript
type Row = { [key: string]: string };
```

하지만 `key`가 `string`이라는 것은 너무 광범위하다. 때문에 광범위한 범위를 좁히는 것이 좋다.

이를 위한 방법을 알아보자.

### 6-1. Record

`Record`는 다음과 같은 형태를 가지고 있다.

```javascript
// lib.es5.d.ts 에서의 정의
type Record<K extends keyof any, T> = {
    [P in K]: T;
};


type Record<K extends string | number | symbol, T> = { [P in K]: T; }
```

먼저 `,`를 기준으로 왼쪽이 `key`, 오른쪽이 `value`이다. 그리고 `K`의 값은 `string`, `number`, `symbol` 중 하나일 수도 있고 부분 집합일 수도 있다.

이러한 `Record`를 사용하여 인덱스 시그니처를 구현하면 아래와 같다.

```javascript
type Row = Record<string, string>;
```

매우 간단하다. 하지만 아직 아쉬운 점은 남아있다. 키의 값이 `string`이기 때문에 아직 광범위하다. 이를 조건에 맞게 바꾸어 보자.

조건엔 a, b, c, d, e 키가 있다고 하였으니 `string` 대신 유니온으로 표현해보자.

```javascript
type Row = Record<'a' | 'b' | 'c' | 'd' | 'e', string>;
// type Row1 = {
//   a: string;
//   b: string;
//   c: string;
//   d: string;
//   e: string;
// };
```

`'a' | 'b' | 'c' | 'd' | 'e'`는 `string`의 부분 집합이므로 가능하다.

하지만 아직 아쉬운 점이 있다. 모두 필수값이라는 것이다. 이를 해결하기 위해선 앞에 `Partial`를 붙이면 된다.

```javascript
type Row = Partial<Record<'a' | 'b' | 'c' | 'd' | 'e', string>>;
// type Row = {
//     a?: string | undefined;
//     b?: string | undefined;
//     c?: string | undefined;
//     d?: string | undefined;
//     e?: string | undefined;
// }
```

이로써 `string`이라는 광범위한 범위를 좁히면서 조건의 타입을 만족할 수 있다.

> `string`이라는 광범위한 범위를 유니온을 사용함으로써 좁히면 더 이상 인덱스 시그니처라고 부를 수 없는게 아닐까?

### 6-2. 매핑된 타입

먼저 다음은 매핑된 타입을 사용한 인덱스 시그니처이다.

```javascript
type Row = { [k in string]: string };
```

사실 매핑된 타입을 좀 더 쉽게 사용하는 방법이 `Record`이다. 하지만 매핑된 타입을 사용하면 더 유연하게 타입을 정할 수 있다.

예를 들어 위의 a, b, c, d, e의 키 중 `d`는 `string`이 아니라 `number`라고 가정해보자. 이는 다음과 같이 코드를 작성하면 된다.

```javascript
type Row = {
  [k in 'a' | 'b' | 'c' | 'd' | 'e']: k extends 'd' ? number : string;
};
```

---

📅 2023-03-21
