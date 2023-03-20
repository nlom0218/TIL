# 아이템 15- 동적 데이터에 인덱스 시그니처 사용하기

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

---

## 4. 그렇다면 인덱스 시그니처는 언제?

가능하다면 인덱스 시그니처보다 정확한 타입을 사용하는 것이 좋다. 하지만 특별한 경우엔 인덱스 시그니처를 사용해야 한다. 바로 런타임 때까지 객체의 속성을 알 수 없을 경우이다.

정확한 속성의 이름을 알지 못하기 때문에 미리 타입으로 정할 수 없기 때문이다.
