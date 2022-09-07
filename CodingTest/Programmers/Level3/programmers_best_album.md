# 베스트 앨범

## 1. 개요

- 프로그래머스
- Lv.3
- 해시
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42579)

---

## 2. 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

- 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
- 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
- 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

---

### 2-1. 문제 설명 - 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

---

### 2-2. 문제 설명 - 입출력 예

| genres                                          | plays                      | return       |
| ----------------------------------------------- | -------------------------- | ------------ |
| ["classic", "pop", "classic", "classic", "pop"] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |

---

### 2-3. 문제 설명 - 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

- 장르 별로 가장 많이 재생된 노래를 최대 두 개까지 모아 베스트 앨범을 출시하므로 2번 노래는 수록되지 않습니다.

---

## 3. 문제 풀이

```javascript
function solution(genres, plays) {
  // 1) 두 개의 맵 객체 생성
  const map = new Map();
  const most_map = new Map();

  // 2) genres 배열의 길이 만큼 반복문 실행
  for (let i = 0; i < genres.length; i++) {
    // 3) map 맵 객체에 저장된 원소 중 키가 genres[i]인 원소의 값을 value에 할당하기
    let value = map.get(genres[i]);

    // 4) value의 값에 따라 새로운 데이터를 추가하기
    if (value) {
      value.push([i, plays[i]]);
    } else {
      value = [[i, plays[i]]];
    }

    // 5) map 맵 객체에 원소 저장하기
    map.set(
      genres[i],
      value.sort((a, b) => {
        if (b[1] - a[1] === 0) {
          return a[0] - b[0];
        } else {
          return b[1] - a[1];
        }
      })
    );

    // 6) most_map 맵 객체에 저장하기
    most_map.set(genres[i], (most_map.get(genres[i]) | 0) + plays[i]);
  }

  // 7) most_map 맵 객체의 크기 만큼 반복문을 실행하며 값이 큰 원소부터 정렬된 배열 만들기
  const most_arr = [];
  for ([key, value] of most_map) {
    most_arr.push([key, value]);
  }
  most_arr.sort((a, b) => b[1] - a[1]);

  // 8) 각 장르 중 많이 재생된 노래 두 곡을 answer 배열에 저장하기
  const answer = [];
  most_arr.forEach(([key, value]) => {
    const [first, second] = map.get(key);
    answer.push(first[0]);
    if (second) {
      answer.push(second[0]);
    }
  });

  return answer;
}
```

---

### 1) 두 개의 맵 객체 생성

```javascript
const map = new Map();
const most_map = new Map();
```

맵 객체를 두 개 생성한다.

- map: 키는 장르이며 값은 `[노래의 고유번호, 재생 횟수]`의 데이터이다. 이때 노래는 키의 장르에 속해있다.
- most_map: 키는 장르이며 값은 해당 장르에 해당하는 노래들의 전체 재생 횟수이다.

---

### 2) genres 배열의 길이 만큼 반복문 실행

```javascript
for (let i = 0; i < genres.length; i++) {}
```

`genres` 배열의 길이 만큼 반복문을 실행한다. 해당 반복문에서 `map` 맵 객체와 `most_map` 맵 객체의 원소가 채워진다.

---

### 3) map 맵 객체에 저장된 원소 중 키가 genres[i]인 원소의 값을 value에 할당하기

```javascript
let value = map.get(genres[i]);
```

`map` 객체 중 `genres[i]`를 키로 가지는 원소의 값을 가져와 `value`에 할당한다. `genres[i]`는 중복 될 수 있다.

---

### 4) value의 값에 따라 새로운 데이터를 추가하기

```javascript
if (value) {
  value.push([i, plays[i]]);
} else {
  value = [[i, plays[i]]];
}
```

만약 `value`가 없다면 아직 해당 장르에는 어떤 노래도 저장이 되어 있지 않다는 것이다. 그렇기 때문에 처음으로 데이터를 넣어주어야 한다.
하지만 만약 `value`가 있다면 배열의 값을 가지고 있으며 `Array.push()` 메서드를 사용하여 데이터를 추가한다.

데이터의 형태는 `[i, plays[i]]`이다. 노래의 고유 번호와 노래의 재생 횟수를 원소로 가지는 배열이다.

---

### 5) map 맵 객체에 원소 저장하기

```javascript
map.set(
  genres[i],
  value.sort((a, b) => {
    if (b[1] - a[1] === 0) {
      return a[0] - b[0];
    } else {
      return b[1] - a[1];
    }
  })
);
```

맵 객체 중복되는 키를 가질 수 없다. 그렇기 때문에 마지막으로 추가된 `[키, 값]`이 맵 객체에 저장된다. 즉 해당 과정을 통해
키가 중복되면 새로운 `[키, 값]`이 저장되고 이전 값은 삭제된다.

또한 해당 과정에서 맵 객체의 값은 재생 횟수를 기준으로 내림차순 정렬을 한다.
하지만 만약 재생 횟수가 같다면 고유 번호가 빠른 순서로 정렬한다.

---

### 6) most_map 맵 객체에 저장하기

```javascript
most_map.set(genres[i], (most_map.get(genres[i]) | 0) + plays[i]);
```

`most_map` 맵 객체에 `key`를 이용하여 `value`를 저장한다.

기존에 `key`가 있다면 해당 값에 노래 재생 횟수를 뜻 하는 `plays[i]`를 더해 `value`를 갱신한다.
처음이라면 `plays[i]`가 `value`가 된다. 계속 누적하여 더해 특정 장르에 속해 있는 모든 노래의 재생 횟수를 구한다.

---

### 7) most_map 맵 객체의 크기 만큼 반복문을 실행하며 값이 큰 원소부터 정렬된 배열 만들기

```javascript
const most_arr = [];
for ([key, value] of most_map) {
  most_arr.push([key, value]);
}
most_arr.sort((a, b) => b[1] - a[1]);
```

`most_map` 맵 객체에 대해 반복문을 실행한다. 반복문을 통해 `most_arr` 배열이 생성이 되는데 `most_arr` 배열의
요소는 `[장르, 장르에 속한 모든 노래의 재생 횟수]`의 형태를 가진다.

`most_arr` 배열이 만들어 진 후 `Array.sort()` 메서드를 사용하여 재생 횟수를 기준으로 내림차순 정렬을 한다.

---

### 8) 각 장르 중 많이 재생된 노래 두 곡을 answer 배열에 저장하기

```javascript
const answer = [];
most_arr.forEach(([key, value]) => {
  const [first, second] = map.get(key);
  answer.push(first[0]);
  if (second) {
    answer.push(second[0]);
  }
});
```

`most_arr`는 총 재생 횟수가 많은 장르 순으로 정렬되어 있다. 처음 요소부터 반복문을 통해 해당 요소를
키로 가지고 있는 `map` 맵 객체의 값을 가져와 구조 분해 할당으로 첫 번째 요소와 두 번째 요소를 선언한다.
첫 번째 요소는 무조건 존재하여 `answer` 배열에 바로 저장할 수 있지만
두 번째 요소는 존재하지 않을 수도 있으므로 존재할 때만 `answer` 배열에 저장한다.

---

### 결과

![programmers_best_album_result1](/image/CodingTest/programmers_best_album/programmers_best_album_result1.png)

---

## 4. Refactoring

두 개의 맵 객체를 사용했지만 이를 하나로 줄이고 싶었다.

```javascript
function solution(genres, plays) {
  const map = new Map();
  genres.forEach((item, index) => {
    let { songs, total } = map.get(genres[index]) || { songs: [], total: 0 };
    songs.push([index, plays[index]]);
    songs.sort((a, b) => {
      if (a[1] === b[1]) {
        return a[0] - b[0];
      }
      return b[1] - a[1];
    });
    total += plays[index];
    map.set(item, { songs: songs.slice(0, 2), total });
  });

  const all_data = [];
  for ([_, value] of map) {
    all_data.push(value);
  }

  return all_data
    .sort((a, b) => b.total - a.total)
    .flatMap((item) => item.songs)
    .map((item) => item[0]);
}
```
