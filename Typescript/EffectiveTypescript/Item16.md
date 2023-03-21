# 아이템 16 - number 인덱스 시그니처보다는 Array, 튜플, ArrayLike를 사용하기

## 1. 자바스크립트에서의 배열

### 1-1. 배열은 Object이다.

```javascript
const array = [1, 2, 3];

typeof array; // Object
```

자바스크립트에서 배열의 타입은 `Object`이다.

### 1-2. Object은 number 타입으로 접근이 불가능하다.

자바스크립트에서의 객체는 `키-값` 형태를 가지고 있으며 `키`는 보통 string(또는 심벌) 타입만 가능하다. 때문에 아래의 예시처럼 기존에 우리가 자주 배열의 값에 접근하기 위해 사용했던 방법은 불가능하다.

```javascript
const array = [1, 2, 3];

array[0];
array[1];
array[2];
```

하지만 number 타입의 키로도 접근하는 것에 있어 오류가 나지 않는 이유는 자바스크립트 엔진에서 자동으로 string 타입으로 형변환이 되기 때문이다.

---

## 2. 일관성을 지닌 타입스크립트

우리는 자바스크립트에서 범용적으로 number 타입의 키를 통해 배열의 값에 접근을 했었기 때문에 타입스크립트에서도 일관성을 위해 number 타입의 키를 허용한다.

`Array`에 대한 타입 선언의 가장 아래에 다음과 같은 것을 확인할 수 있다.

```javascript
interface Array<T> {
  // ...
  [n: number]: T;
}
```

다시 말해, 실제 `Object`에서는 문자열(혹은 심벌) 타입으로만 값에 접근이 가능하기 때문에 `[n: string]: T`가 올바른 표현이지만 자바스크립트와의 일관성을 위해 키 타입을 `number`로 정한 것이다.

---

## 3. number 인덱스 시그니처 살펴보기

간단한 number 인덱스 시그니처를 살펴보자.

```javascript
type Animals = { [n: number]: string };

const animals: Animals = {
  0: 'cat',
  1: 'dog',
};
```

위 Animals 타입의 키는 `number`타입 이고 값은 `string` 타입이다. 때문에 animals 객체는 아래와 같이 접근이 가능하다.

```javascript
animals[0];
animals[1];
```

---

## 4. number 인덱스 시그니처 대신 Array 또는 튜플 타입 사용하기

굳이 number 인덱스 시그니처를 사용하여 키 타입이 `number`인 객체를 만들 필요가 있을까? 이미 있는 타입인 `Array` 타입을 사용하여 number 인덱스 시그니처를 대신 할 수 없을까? 정답은 할 수 있다.

### 4-1. Array 타입 사용

```javascript
const animals: Array<string> = ['cat', 'dog', 'tiger'];
```

값이 `[]`에 속해있냐, `{}`에 속해있냐는 다르다. 하지만 접근하는 관점에 본다면 이 둘은 같다. 즉, 다음과 같이 접근할 수 있다.

```javascript
animals[0];
animals[1];
animals[2];
```

### 4-2. 튜플 타입 사용

또는 값의 개수가 정해져 있다면 `튜플` 타입도 사용할 수 있다.

```javascript
const foods: [string, string] = ['pizza', 'noodle'];

foods[0];
foods[1];
```

### 4-3. 결론

number 인덱스 시그니처가 필요하다면 `Array`, `튜플` 타입을 사용하면 된다. 이미 타입스크립트에 있는 타입을 활용하는 것이기 때문에 타입스크립트를 더 잘 활용하는 것이지 않을까 싶다.

하지만 문제는 여전히 존재한다.

`Array`, `튜플` 타입은 모두 배열이 기본적으로 가지고 있는 프로퍼티와 메서드가 모두 존재한다. 하지만 우린 값에 `접근`만 하고 싶을 뿐이다. 또는 길이 정도만...?

![쓸모없는 여러 메서드들](/image/Typescript/EffectiveTypescript/item16-1.png)

이런 문제는 어떻게 해결할 수 있을까?

---

## 5. number 인덱스 시그니처 대신 ArrayLike 타입 사용하기

위 문제는 `ArrayLike` 타입을 사용하여 해결할 수 있다. `ArrayLike` 타입의 형태를 보면 다음과 같다.

```javascript
interface ArrayLike<T> {
  readonly length: number;
  readonly [n: number]: T;
}
```

`length` 프로퍼티와 `키-값`만 존재한다. `ArrayLike` 타입을 사용해보도록 하자.

```javascript
const animals: ArrayLike<string> = ['cat', 'dog', 'tiger'];

animals[0]; // cat
animals[1]; // dog
animals.length; // 3
```

배열의 형태로 값을 넣을 수 있고 객체의 형태로 값을 넣을 수 있다.

> 참고로 `Array` 타입도 객체로 값을 넣을 수 있지만 모든 메서드의 값을 정해야 하기 때문에 불가능에 가깝다. 귀찮다.

```javascript
const animals: ArrayLike<string> = {
  0: 'cat',
  1: 'dog',
  2: 'tiger',
  length: 3,
};
```

객체의 형태로 값을 넣을 땐 `length` 속성도 함께 넣어줘야 한다.

---

## 6. 그렇다면 언제?

지금까지 살펴본 내용을 언제 적용하면 좋을지 생각하면 아래와 같다.

1. 정말 인덱스 시그니처가 필요한 상황인가? - Item 15를 참고
2. 키 타입이 `number`이어야 하는가?
3. 만약 그렇다면 새로운 타입을 만들지 말고 `Array`, `튜플`, `ArrayLike` 타입을 사용하자.

결론! 웬만하면 사용할 일이 없을 듯!

---

📅 2023-03-21
