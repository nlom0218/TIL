# 아이템 7 - 타입은 값들의 집합이다.

## 1. 타입은 할당 가능한 값들의 집합

자바스크립트에서는 변수에 다양한 값을 할당할 수 있다. 이러한 값들을 타입스크립트가 런타임 이전에 `여기에 할당할 수 있는가?`라는 의문을 품고 오류를 체크하기 시작한다.

오류를 통해 타입스크립트에서의 집합이라는 개념에 다가가보자.

### 1-1. 타입 오류1

타입스크립트가 만약 오류를 발견하면 아래와 같은 오류를 발견할 수 있다.

![타입 오류](/image/Typescript/EffectiveTypescript/item7-1.png)

`Type 'number' is not assignable to type 'string'.` 숫자 타입은 문자열에 타입에 할당을 할 수 없다라는 것이다. 이 문구는 집합의 관점에서 아래처럼 해석할 수 있다.

- 숫자 타입은 문자열 타입의 `부분 집합`이 아니다.(두 타입의 관계)

### 1-2. 타입 오류2

또 다른 오류를 보며 집합이라는 개념을 적립해보자.

![타입 오류](/image/Typescript/EffectiveTypescript/item7-2.png)

`AB`의 타입은 "A" 또는 "B"이다. 하지만 그 이 외의 값을 할당했기 때문에 위와 같은 오류가 나타난다. 이 문구도 집합의 관점에서 해석해보면 아래와 같다.

- "C"타입은 "AB" 타입의 원소가 아니니다.(값과 타입의 관계)

### 1-3. 그래서 집합이란?

```javascript
type Person = {
  name: string,
  age: number,
};
```

`Person`의 속성인 name, age가 집합이 아니라, `Person` 그 자체가 집합이다.

---

## 2. 타입스크립트에서의 타입(집합)의 종류

### 2-1. never 타입

`never` 타입은 타입스크립트에서 가장 작은 직합이다. `never` 타입으로 선언된 변수의 범위는 공집합이기 때문에 아무런 값도 할당할 수 없다.

### 2-2. 유닛(unit) 타입

`never` 타입 다음으로 작은 집합은 한 가지 값만 포함하는 타입이다. 이를 `유닛(unit) 또는 리터럴(literal)` 타입이라고 한다.

```javascript
type A = 'A';
type B = 'B';
type Twelve = 12;
```

### 2-3. 유니온(union) 타입

`유닛(unit)` 타입을 여러 개 묶으면 `유니온(union)` 타입이 된다. 유니온 타입은 값 집합들의 합집합을 뜻한다.

```javascript
type AB = 'A' | 'B';
type Animals = 'Cat' | 'Dog' | 'Bird';
```

### 2-4. 그 외의 타입들

흔히 쓰이는 `number`, `string`, `boolean` 등... 이들도 하나의 집합이다. 즉, 동일한 특징을 묶은 하나의 집합이다.

---

## 3. 타입스크립트에서 집합간의 관계

위에서 타입스크립트에서의 타입은 `할당 가능한 값들의 집합`이라는 것을 살펴보았다. 또한 `유니온(union)` 타입은 값 집합들의 합집합이라고 하였다. 이를 좀 더 살펴보자.

중고등 수학에서 배운 집합의 관계를 일단 머리속에서 지워야 한다. 수학에서 집합은 아래와 같이 사용되었다.

- A: {1, 2, 3}
- B: {3, 4, 5}
- A ∩ B: {3}
- A ∪ B: {1, 2, 3, 4, 5}

수학에서의 교집합은 공통으로 가지는 원소이고 합칩합은 두 집합의 모든 원소를 뜻한다. 하지만 타입스크립트에서의 집합은 이와는 다르다. 타입스크립트에서의 타입과의 관계를 수학처럼 굳이 표현하자면 아래와 같다.

- A: {1, 2, 3}
- B: {3, 4, 5}
- A ∩ B: {1, 2, 3, 4, 5}
- A ∪ B: {1, 2, 3} | {3, 4, 5}

지금봐도 혼란스럽다.

교집합, 합집합을 차례대로 살펴보자.

### 3-1. 타입스크립트에서의 교집합

타입스크립트에서 교집합을 나타내기 위한 기호는 `&`이고 이는 `인터섹션`이라 불린다.

위의 예시를 코드로 살펴보자.

```javascript
type A = {
  a: number,
};

type B = {
  b: number,
};

type C = A & B;
```

타입 C는 타입 A와 타입 B의 교집합니다. 그렇다면 타입 C는 어떤 속성을 가지고 있을까? 만약 수학에서의 집합으로 접근한다면 타입 A와 타입 B의 속성 중에서는 어느 것도 겹치는 것이 없기 때문에 공집합으로 생각할 것이다. 하지만 이는 틀렸다.

결론부터 말하면 타입 C는 아래와 같은 속성을 가진다.

```javascript
type C = {
  a: number,
  b: number,
};
```

위를 바라보고 있자면,, 마치 타입 A와 타입 B의 합집합 처럼 느껴진다. 아마 이런 이유는 타입의 속성을 기준으로 생각했기 때문일 것이다. 이번엔 보는 관점을 다르게 해보자. `A`, `B`가 하나의 집합이다. 이 두개의 집합을 만족하는 것은 `{a: number, b: number}`이다. 타입의 속성이 더 많다는 것은 더 작은 집합이다. 이것을 타입스크립트를 쓰면서 항상 생각하자.

### 3-2. 타입스크립트에서의 합집합

이는 위에서 `유니온(union)` 타입에서 살펴보았다. `|`를 사용하여 합집합을 나타내며 그들 중 하나만 만족하면 된다. 보통 리터럴 타입을 여러개 묶을 때 사용된다.

리터럴 타입을 유니온으로 사용하는 것은 이애가 되지만 리터럴이 아니라 커스튬 타입과 쓰이는 유니온에 대해 궁금증이 생겨 여러 크루들과 이야기를 나누어 보았다. 문제 상황은 아래와 같다.

```javascript
type Person = {
  name: string,
};

type AddressAndAge = {
  age: number,
  address: string,
};

type PersonWithAddreeAndAge = Person | Address;
```

`PersonWidthAddressAndAge` 타입은 `Person` 타입과 `Address` 타입의 유니온이다. 즉 두 타입의 합집합이고 두 타입의 교집합은 해당되지 않는다. 예를들어 아래와 같은 타입은 `PersonWidthAddressAndAge` 타입에 해당되지 않는다.

```javascript
const person: PersonWithAddreeAndAge = {
  name: 'hd',
  age: 12,
  address: 'something..',
};
```

But...! 위의 타입에서는 에러가 나지 않는다. 하지만 해당 타입으로 선언한 객체에 접근을 하려고 보니 에러가 나타난다. 아직 해당 이유에 대해 확실한 답을 찾지 못하였다. 추측컨데 타입가드를 의도하지 않았나 싶다.

아무튼 유니온은 커스튬 타입에서는 사용하지 말고 리터럴 타입에 대해서만 사용하도록 하자.

---

## 4. 제너릭 타입에서의 extends

`extends` 키워드는 제너릭 타입에서 한정자로 쓰이며, `~의 부분 집합`을 의미한다. 예를 들어 아래와 같은 제너릭 타입이 있다.

```javascript
function getKey<K extends string>(val: any, key: K) {}
```

두번째 인자인 `key`의 타입은 `K` 타입이다. 즉 제너릭 타입으로 이는 앞에서 더 정확한 뜻을 알 수 있다. `<K extends string>`을 통해 `K` 타입은 `string`과 어떠한 연관이 있어 보인다. 그 연관은 바로 `K`는 `string의 부분 집합`을 만족해야 한다는 것이다. 즉, `K` 타입은 string 리터럴 타입, string 리터럴 타입의 유니온, string 를 포함한다. 때문에 아래와 같이 함수를 호출하면 오류가 나타날 것이다.

```javascript
getKey({}, 12);
```

---

## 5. 배열과 튜플 그리고 트리플

튜플은 두 숫자를 가지는 타입을 말한다. 이는 배열의 부분 집합으로 볼 수 있기 때문에 튜플은 배열에 할당할 수 있다.

```javascript
type Numbers = number[];

const tuple: [number, number] = [1, 2];
const numbers: Numbers = tuple;
```

하지만 그 반대로, 배열은 튜플의 부분 집합이 아니므로 배열은 튜플에 할당할 수 없다.

```javascript
type Tuple = [number, number];

const list = [1, 2]; // type은 number[]
const numbers: Tuple = list; // Type 'number[]' is not assignable to type 'Tuple'.
```

트리플은 세 숫자를 가지는 타입으로 구조적 타이핑의 관점으로 본다면 튜플에 할당 가능하다고 생각된다. 하지만 그렇지 않다.

> 구조적 타이핑으로 생각한다면 튜플보다 더 많은 인자를 가지고 있기 때문에 튜플을 만족할 수 있다고 생각할 수 있다.

```javascript
type Tuple = [number, number];

const triple: [number, number, number] = [1, 2, 3];
const tuple: Tuple = triple; // Type '[number, number, number]' is not assignable to type 'Tuple'.
// Source has 3 element(s) but target allows only 2.
```

위와 같은 오류가 나타나는 이유는 튜플과 트리플 같은 타입은 `length`라는 값을 가지고 있기 때문에 때문에 `length`의 값이 서로 다르기 때문에 오류가 발생한 것이다.

---

## 6. 타입스크립트 타입이 되지 못하는 값의 집합

타입스크립트에서는 타입이 되지 못하는 값의 집합들이 있다. `정수에 대한 타입`, `x와 y 속성 외에 다른 속성이 없는 객체`는 타입스크립트 타입에 존재하지 않는다.

가령 `Exclude` 키워드를 사용하여 일부 타입을 제외할 때, 타입스크립트 타입이 되지 못하는 값의 집합을 제외할 수 없는 것이다.

```javascript
type NonZeroNums = Exclude<number, 0>;
```

`NonZeroNums` 타입은 여전히 `number`이다.

---

📅 2023-03-17
