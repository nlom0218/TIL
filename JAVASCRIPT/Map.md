# Map

## 1. 개요

ES6부터 도입된 자바스크립트의 객체인 맵(Map)과 셋(Set)은 기존의 자료구조인 객체와 배열과 비슷하지만 차이점이 분명 존재합니다. 이번 챕터에서는 객체와 배열과 다른 맵과 셋의 특징을 알아보자.

---

## 2. Object vs Map

- Object의 키는 String이며, Map은 문자열이 아닌 값도 키로 설정할 수 있다.
- Map은 크기를 간단한 방법으로 가져올 수 있다.
- Map은 추가된 순서대로 반복한다.

---

## 3. 맵(Map) 객체 생성

맵은 키가 있는 데이터를 저장한다는 점에서 객체와 유사하지만 맵은 키에 다양한 자료형을 허용한다는 점에서 차이가 있다.

먼저 맵 객체 생성하는 방법에 대해 알아보자.

### 3-1. 빈 Map 객체 생성

```js
let map = new Map();
```

![map_obj_create](/image/JS/MapSet/map_object_create1.png)

---

### 3-2. [키, 값] 형태의 중첩 배열을 전달하여 Map 객체 생성

1.  기본 생성 방법

    ```js
    let map = new Map([
      ["A", "valueA"],
      ["B", "valueB"],
      ["C", "valueC"],
    ]);
    ```

    ![map_obj_create 2](/image/JS/MapSet/map_object_create2.png)

2.  만약, 중복되는 키가 존재할 경우 마지막 값이 적용된다.

    ```js
    let map = new Map([
      ["A", "valueA"],
      ["B", "valueB"],
      ["C", "valueC"],
      ["B", "valueBB"],
    ]);
    ```

    ![map_object_create 2](/image/JS/MapSet/map_object_create3.png)

3.  [키, 값]에서 값은 객체가 될 수 있다.

    ```js
    let map = new Map([
      ["A", { value: "valueA" }],
      ["B", { value: "valueB" }],
      ["C", { value: "valueC" }],
    ]);
    ```

    ![map_object_create 4](/image/JS/MapSet/map_object_create4.png)

4.  [키, 값]에서 키도 객체가 될 수 있지만 고유한 값이 될 수 없다.

    ```js
    let map = new Map([
      [{ key: "A" }, { value: "valueA" }],
      [{ key: "B" }, { value: "valueB" }],
      [{ key: "C" }, { value: "valueC" }],
      [{ key: "B" }, { value: "valueBB" }],
    ]);
    ```

    ![map_object_create 5](/image/JS/MapSet/map_object_create5.png)

---

### 3-3. Map.set(key, value): key를 이용해 value를 저장

1. 기본 사용 방법

   ```js
   let map = new Map();
   map.set("A", "valueA");
   map.set("B", "valueB");
   map.set("C", "valueC");
   ```

   ![map_object_create 6](/image/JS/MapSet/map_object_create6.png)

2. 체이닝 방법

   `map.set`을 호출할 때마다 맵 자신이 반환된다. 이를 이용하여 `map.set`을 체이닝(chaining)할 수 있다.

   ```js
   let map = new Map();
   map.set("A", "valueA").set("B", "valueB").set("C", "valueC");
   ```

   ![map_object_create 7](/image/JS/MapSet/map_object_create7.png)

---

### 3-4. Object.entries(obj): 평범한 객체를 가지고 맵(Map) 만들기

평범한 객체를 가지고 `맵(Map)`을 만들고 싶으면 내장 메서드 `Object.entries(obj)`를 활용해야 한다. 이 메서드는 객체의 키-값 쌍을 요소(`[key, value]`)로 가지는 배열을 반환한다.

예를 들어 아래와 같은 평범한 객체가 있다.

```js
const obj = {
  name: "HD",
  age: 29,
  gender: "male",
};
```

위의 `obj`객체를 `Object.entries(obj)`의 인자로 넣게 되면 키-값 쌍을 요소(`[key, value]`)로 가지는 배열을 반환한다. (아래의 코드 및 사진 참고)

```js
const entries = Object.entries(obj);
```

![map_object_create 8](/image/JS/MapSet/map_object_create8.png)

`entreis`은 각 요소가 [키, 값] 쌍인 배열이다. 그러므로 `new Map()`에 전달하여 새로운 맵을 만들 수 있다.

```js
const map = new Map(entries);
```

![map_object_create 9](/image/JS/MapSet/map_object_create9.png)

---

## 4. Map의 주요 메서드와 프로퍼티

### 4-1. Map.get(key)

`key`에 해당하는 값을 반환한다. `key`가 존재하지 않으면 `undefined`를 반환한다.

```js
let map = new Map();
map.set("name", "HD").set("age", 29);

console.log(map.get("name"));
console.log(map.get("gender"));
```

![map_get](/image/JS/MapSet/map_get.png)

> `map`을 사용할 땐 `map`전용 메서드 `set`, `get` 등을 사용하자. 기존 객체에서 사용했던 방법은 `map`을 일반 객체처럼 취급하게 되어 여러 제약이 생긴다.

---

### 4-2. Map.has(key)

`key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

console.log(map.has("A"));
console.log(map.has("C"));
console.log(map.has("F"));
```

![map_has](/image/JS/MapSet/map_has.png)

---

### 4-3. Map.delete(key)

`key`에 해당하는 값을 삭제한다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

console.log(map.has("C"));
map.delete("C");
console.log(map.has("C"));
```

![map_delete](/image/JS/MapSet/map_delete.png)

---

### 4-4. Map.clear()

맵 안의 모든 요소를 제거한다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

map.clear();
console.log(map);
```

![map_clear](/image/JS/MapSet/map_clear.png)

---

### 4-5. Map.size

요소의 개수를 반환한다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

console.log(map.size);
```

![map_size](/image/JS/MapSet/map_size.png)

---

### 4-6. Map.keys()

각 요소의 키를 모은 반복 가능한(iterable, 이터러블) 객체를 반환한다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

console.log(map.keys());
```

![map_keys](/image/JS/MapSet/map_keys.png)

---

### 4-7. Map.values()

각 요소의 값을 모든 이터러블 객체를 반환한다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

console.log(map.values());
```

![map_values](/image/JS/MapSet/map_values.png)

---

### 4-8. Map.entries()

요소의 `[키, 값]`을 한 쌍으로 하는 이터러블 객체를 반환한다. 이 이터러블 객체는 `for...of`반복문의 기초로 쓰인다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

console.log(map.entries());
```

![map_entries](/image/JS/MapSet/map_entries.png)

아래는 `for...of`반복문을 사용하여 [키, 값] 쌍을 대상으로 순회하는 코드이다.

```js
for ([key, value] of map.entries()) {
  console.log(key, value);
}
for ([key, value] of map) {
  console.log(key, value);
}
```

두 반복문의 결과는 같다.

![map_entries 2](/image/JS/MapSet/map_entries2.png)

> 맵은 배열과 유사하게 내장 메서드 `forEach`도 지원한다.
>
> ```js
> let map = new Map([
>   ["A", "valueA"],
>   ["B", "valueB"],
>   ["C", "valueC"],
>   ["D", "valueD"],
>   ["E", "valueE"],
> ]);
> map.forEach((value, index) => {
>   console.log("value: ", value);
>   console.log("index: ", index);
> });
> ```
>
> ![map_forEach](/image/JS/MapSet/map_forEach.png)

---

### 4-9. Object.fromEntries

맵을 객체로 바꾸기 위해서는 `Object.fromEntries`를 사용하면 된다. 이 메서드는 각 요소가 `[키, 값]` 쌍인 배열을 객체로 바꿔준다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);

// map.entries()를 호출하면 맵의 [키, 값]을 요소로 가지는 이터러블을 반환한다.
// 이런 과정을 거치는 이유는 Object.fromEntries 메서드를 사용하기 위함이다.
const entries = map.entries();
console.log(entries);

const obj = Object.fromEntries(entries);
console.log(obj);
```

![map_fromEntries](/image/JS/MapSet/map_fromEntries.png)

하지만 굳이 `map.entries()`를 하지않고 `Object.fromEntries()` 메서드의 인자로 맵(Map)을 전달해도 된다.

`Object.fromEntries`는 인자로 이터러블 객체를 받기 때문에 꼭 배열을 전달해줄 필요는 없다.(맵도 이터러블 객체이다.)

즉, 아래와 같이 위의 코드를 수정할 수 있다.

```js
let map = new Map([
  ["A", "valueA"],
  ["B", "valueB"],
  ["C", "valueC"],
  ["D", "valueD"],
  ["E", "valueE"],
]);
const obj = Object.fromEntries(map);
```

---

## 5. Conclusion

> 옛날 `모던 자바스크립트 Deep Dive`책을 보며 공부했던 적이 있다. 그 책에 나왔던 내용 중 하나가 바로 오늘 공부한 맵(Map)에 대해 봤던 기억이 있다. 당시에는 정말 대~~충 보고 넘겼기 때문에 오늘 공부하면서도 많이 생소하게 느꼈다. 또한 이터러블와 같은 개념도 오랜만에 보는 것이기에 머리속이 복잡해졌다. `모던 자바스크립트 Deep Dive`책을 다시 보면서 자바스크립트의 기본기를 탄탄히 다질 수 있도록 하자. 처음부터 다시 보며 하나하나 내 것으로 만들어 보자! 아마 그 땐 알던 내용보다는 자바스크립트가 작동하는 원리, 생소한 개념을 먼저 정리하며 공부하지 않을까 싶다.  
> 맵에 대한 느낌은 반복가능한 객체이므로 `Array.forEach()`메서드를 사용할 수 있다는 점이 신기하였다. 당연히 `for...of`구문도 사용하니 코테를 풀고 공부할 때 맵을 잘 활동하도록 해야겠다.

---

## 참고

[맵과 셋](https://ko.javascript.info/map-set#ref-3157)  
[[JavaScript]Map 객체](https://developer-talk.tistory.com/170)

---

[👆](#map)

📅 2022-08-08
