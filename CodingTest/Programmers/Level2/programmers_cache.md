# 캐시

## 1. 개요

- 프로그래머스
- Lv.2
- 2018 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/17680)

---

## 2. 문제 설명

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.  
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.  
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

---

### 2-1. 문제 설명 - 입력 형식

- 캐시 크기(cacheSize)와 도시이름 배열(cities)을 입력받는다.
- cacheSize는 정수이며, 범위는 0 ≦ cacheSize ≦ 30 이다.
- cities는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
- 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

---

### 2-2. 문제 설명 - 출력 형식

- 입력된 도시이름 배열을 순서대로 처리할 때, "총 실행시간"을 출력한다.

---

### 2-3. 문제 설명 - 조건

- 캐시 교체 알고리즘은 LRU(Least Recently Used)를 사용한다.
- cache hit일 경우 실행시간은 1이다.
- cache miss일 경우 실행시간은 5이다.

---

### 2-4. 문제 설명 - 입출력 예제

| 캐시크기(cacheSize) | 도시이름(cities)                                                                                                  | 실행시간 |
| ------------------- | ----------------------------------------------------------------------------------------------------------------- | -------- |
| 3                   | ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "Jeju", "Pangyo", "Seoul", "NewYork", "LA"]                          | 50       |
| 3                   | ["Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul"]                                 | 21       |
| 2                   | ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"] | 60       |
| 5                   | ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"] | 52       |
| 2                   | ["Jeju", "Pangyo", "NewYork", "newyork"]                                                                          | 16       |
| 0                   | ["Jeju", "Pangyo", "Seoul", "NewYork", "LA"]                                                                      | 25       |

---

## 3. 문제 풀이

```javascript
function solution(cacheSize, cities) {
  // 1) cacheSize가 0일 땐 총 도시 수에 cache miss인 5를 곱한 값을 반환하기
  if (cacheSize === 0) return 5 * cities.length;

  // 2) 배열로 캐시 관리하기 & 실행시간을 누적해서 관리하기
  let cache = [];
  let time = 0;
  cities.forEach((item) => {
    const city = item.toLowerCase();

    // 3) 새롭게 추가되는 도시가 이미 캐시에 있는지 확인하기
    const index = cache.indexOf(city);

    // 4) 이미 캐시에 도시가 있다면 가장 마지막 요소로 옮긴 후 실행시간을 1 올리기
    if (index !== -1) {
      const a = cache.slice(0, index);
      const b = cache.slice(index + 1);
      cache = [...a, ...b];
      cache.push(city);
      time += 1;
    } else {
      // 5) 캐시에 도시가 없다면 조건에 따라 도시를 추가하고 실행시간을 5 올리기
      if (cache.length === cacheSize) cache.shift();
      cache.push(city);
      time += 5;
    }
  });
  return time;
}
```

---

### 1) cacheSize가 0일 땐 총 도시 수에 cache miss인 5를 곱한 값을 반환하기

```javascript
if (cacheSize === 0) return 5 * cities.length;
```

`cacheSize`가 0이라는 것은 새로운 도시가 캐시에 추가 될 때 마다 캐시엔 저장되어 있는 도시가 없기 때문에
항상 `cache miss`가 적용된다. 그렇기 때문에 `cicties` 배열의 길이에 `5`를 곱한 값을 반환한다.

---

### 2) 배열로 캐시 관리하기 & 실행시간을 누적해서 관리하기

```javascript
let cache = [];
let time = 0;
```

`cacheSize` 만큼 `cache`가 생성된다. 이런 캐시를 배열로 관리한다. 또한 캐시가 교체 될 때 마다 실행되는
시간을 누적하여 구해야 하는데 이를 위해 `time`이라는 변수를 만들어 관리한다.

---

### 3) 새롭게 추가되는 도시가 이미 캐시에 있는지 확인하기

```javascript
const index = cache.indexOf(city);
```

`cache` 배열에 새롭게 추가 될 도시가 있는지 확인한다. `cache` 배열은 중복된 요소를 가질 수 없기 때문에
새롭게 추가 될 도시가 이미 `cache` 배열에 있다면 해당 인덱스를 반환할 것이고 그렇지 않으면 `-1`를 반환할 것이다.

---

### 4) 이미 캐시에 도시가 있다면 가장 마지막 요소로 옮긴 후 실행시간을 1 올리기

```javascript
if (index !== -1) {
  const a = cache.slice(0, index);
  const b = cache.slice(index + 1);
  cache = [...a, ...b];
  cache.push(city);
  time += 1;
}
```

`Array.slice()` 메서드를 사용하여 `index`에 해당하는 요소만 제거한 새로운 배열을 만든다. 그 후 `cache`의
마지막 요소에 제거한 요소를 다시 넣는다.

`cache`의 총 길이는 변하지 않고 `index`에 해당하는 요소만 맨 뒤로 옮기는 작업이다.

지금 생각하면 `Array.filter()` 메서드를 사용해도 좋았을 것이라 생각한다. `Array.filter()` 메서드를
사용하면 아래와 같이 코드를 작성할 수 있다.

```javascript
cache = cache.filter((item) => item !== city);
```

`cache` 배열의 순서를 바꾼 후 `cache hit`의 경우이기 때문에 `time`를 1 올린다.

---

### 5) 캐시에 도시가 없다면 조건에 따라 도시를 추가하고 실행시간을 5 올리기

```javascript
if (cache.length === cacheSize) cache.shift();
cache.push(city);
time += 5;
```

위와 같은 경우엔 두 가지의 조건을 다시 생각을 해야 한다.

1. `cache` 배열의 길이가 `cacheSize`보다 작을 경우
   - `cache` 배열의 마지막 요소로 도시를 추가한다.
2. `cache` 배열의 길이가 `cacheSize`와 같을 경우
   - `cache` 배열의 첫 번째 요소를 제거한다.
   - `cache` 배열의 마지막 요소로 도시를 추가한다.

새로운 도시가 추가되었기 때문에 `cache miss`의 경우이고 해당 경우엔 `time`를 5 올린다.

---

### 결과

![programmers_cache_result1](/image/CodingTest/programmers_cache/programmers_cache_result1.png)

---

## 4. 다른 사람 풀이

`Map`으로 푼 풀이가 재밌어 보였고 풀이에서 배울 내용이 있기 때문에 가져왔다. 또한 풀이를 그대로 가져온 것이
아니라 내 나름대로 가독성이 좋게 바꾸었다. 굳이 필요없는 매개변수를 삭제하였다.

```javascript
function solution(cacheSize, cities) {
  const map = new Map();

  // 1) cacheHit인 경우 map 수정하기
  const cacheHit = (city) => {
    map.delete(city);
    map.set(city, city);
    return 1;
  };

  // 2) cacheMiss인 경우 map 수정하기
  const cacheMiss = (city) => {
    if (cacheSize === 0) return 5;
    if (map.size === cacheSize) map.delete([...map.keys()][0]);
    map.set(city, city);
    return 5;
  };

  return cities
    .map((city) => {
      // 3) city를 키로 가진 맵 객체가 있는지 확인하기
      const hasCity = map.has(city.toLowerCase());

      // 4) hasCity에 따라 다른 함수 실행하기
      if (hasCity) return cacheHit(city.toLocaleLowerCase());
      else return cacheMiss(city.toLocaleLowerCase());
    })
    .reduce((a, c) => a + c, 0);
}
```

---

### 1) cacheHit인 경우 map 수정하기

```javascript
const cacheHit = (city, map) => {
  map.delete(city);
  map.set(city, city);
  return 1;
};
```

`cacheHit`인 경우 실행되는 함수이다. 즉, 이미 캐시에 도시가 있는 경우 실행된다.
이땐 이미 있던 키가 `city`인 맵 객체를 삭제하고 다시 같은 맵 객체를 생성한다.

이는 맵 객체의 순서를 바꾸는 역할을 한다.

---

### 2) cacheMiss인 경우 map 수정하기

```javascript
const cacheMiss = (city) => {
  if (cacheSize === 0) return 5;
  if (map.size === cacheSize) map.delete([...map.keys()][0]);
  map.set(city, city);
  return 5;
};
```

`cacheMiss`인 경우 실행되는 함수이다. 즉, 캐시에 도시가 없는 경우 실행된다.

먼저 `cacheSize`가 0인 경우 저장되는 맵 객체는 없고 5를 리턴하기만 한다.

그렇지 않고 만약 맵의 `size`와 `cacheSize`가 같다면 먼저 첫 번째 맵 객체를 삭제한다. 그 후 새로운
맵 객체를 생성한다. 이때 생성되는 맵 객체의 키는 매개변수로 받은 `city`가 된다.

그리고 맵의 `size`가 아직 `cacheSize`와 같지 않다면 첫 번째 맵 객체를 삭제하지 않고 새로운 맵 객체만
생성한다.

여기서 배울 점은 `map.keys()` 메서드를 사용하여 첫 번째 맵 객체의 키를 가져온 것이다. 배열로 바꾼 후 `0`번째
인덱스를 가져왔는데 이는 `map.keys()` 메서드가 반환하는 값이 이터러블하기 때문이다.

---

### 3) city를 키로 가진 맵 객체가 있는지 확인하기

```javascript
const hasCity = map.has(city.toLowerCase());
```

`cacheHit`인지 `cacheMiss`인지 구별하기 위해 `map`에 `city`를 키로 가지는 맵 객체가 있는지 확인한다.

---

### 4) hasCity에 따라 다른 함수 실행하기

```javascript
if (hasCity) return cacheHit(city.toLocaleLowerCase());
else return cacheMiss(city.toLocaleLowerCase());
```

만약 `hasCity`가 `true`라면 `map`에 이미 도시가 있기 때문에 `cacheHit` 함수가 실행되고 그렇지 않으면
`cacheMiss`함수가 실행된다.

---

### 결과

![programmers_cache_result2](/image/CodingTest/programmers_cache/programmers_cache_result2.png)

---

## 5. Conclusion

> 문제 설명을 읽을 때 캐시 교체 알고리즘인 `LRU`에 대해 처음 알게 되었다. 그래서 검색을 통해 어떤 알고리즘인지
> 파악을 하였다. 다행히 생각보다 어렵지 않는 알고리즘이여서 금방 이해를 하게 되었다. 하지만 문제를 풀기 위해
> 어떤 코드를 작성해야하는지 정확한 감을 잡지 못해 빠른 시간안에 해결을 하지 못했다. 왠지 문제를 푼 당일
> 라인, 카카오 코테가 있었기 때문이라 생각한다. 조급한 마음에 머리보다 손이 더 빨리 움직였던 거 같다.
> 천천히 그리고 확실히 한 번에 풀 수 있도록 하자. 그리고 처음 `LRU`알고리즘을 봤을 때, 큐가 생각이 났지만
> `cacheSize`의 범위가 크지 않아 배열로 풀어도 좋다고 생각을 하였고 또한 단순히 데이터를 빼내는 것 뿐 아니라
> 데이터의 순서도 바꿔야 하지 때문에 배열이 더 알맞는 자료구조라고 생각을 하였다.

---

📅 2022-09-26
