# 버블 정렬(Bubble Sort)

## 1. 버블 정렬이란?

> A sorting algorithm where the largest values bubble up to the top!

버블 정렬은 바로 옆의 값과 비교하여 순서를 바꾸는 것을 반복한다. 예를 들어 숫자를 올림차순으로 정렬을 하고자 할 때, 정렬하고자 하는 값이 바로 다음 값보다 크다면 순서를 바꾸고 그렇지 않으면 순서를 바꾸지 않는다.

데이터가 하나 하나 쌓여간다고 생각하면 된다. 만약 올림차순이라면 가장 큰 값이 먼저 쌓이고 다음으로 큰 값이 쌓인다. 차곡차곡... 추가적으로 쌓인다는 것은 배열의 마지막 인덱스부터 채워나간다는 것!

### 1-1. 버블 정렬의 간단한 예시

배열 `[5, 3, 4, 1, 2]`을 올림차순으로 정렬하고자 한다.

1. 첫번째 정렬하기: 다섯번째 값이 정해진다.
   - `[5, 3, 4, 1, 2]`: 시작
   - `[3, 5, 4, 1, 2]`: 5와 3을 비교 -> 순서를 바꾼다.
   - `[3, 4, 5, 1, 2]`: 5와 4를 비교 -> 순서를 바꾼다.
   - `[3, 4, 1, 5, 2]`: 5와 1을 비교 -> 순서를 바꾼다.
   - `[3, 4, 1, 2, 5]`: 5와 2를 비교 -> 순서를 바꾼다.
2. 두번째 정렬하기: 네번째 값이 정해진다.
   - `[3, 4, 1, 2, 5]`: 시작
   - `[3, 4, 1, 2, 5]`: 3과 4를 비교 -> 순서를 바꾸지 않는다.
   - `[3, 1, 4, 2, 5]`: 4와 1을 비교 -> 순서를 바꾼다.
   - `[3, 1, 2, 4, 5]`: 4와 2를 비교 -> 순서를 바꾼다.
3. 세번째 정렬하기: 세번째 값이 정해진다.
   - `[3, 1, 2, 4, 5]`: 시작
   - `[1, 3, 2, 4, 5]`: 3과 1를 비교 -> 순서를 바꾼다.
   - `[1, 2, 3, 4, 5]`: 3과 2를 비교 -> 순서를 바꾼다.
4. 네번째 정렬하기: 두번째 값이 정해진다.
   - `[1, 2, 3, 4, 5]`: 시작
   - `[1, 2, 3, 4, 5]`: 1과 2를 비교 -> 순서를 바꾸지 않는다.

정렬을 위와 같이 반복한다. 특정 위치의 값이 정해지면 더 이상 해당 위치는 비교하지 않는 특징을 가진다.

---

## 2. 버블 정렬 의사 코드

### 2-1. 최적화되지 않은 의사 코드

```javascript
function bubbleSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      if (arr[j] > arr[j + 1]) [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
    }
  }

  return arr;
}
```

위는 최적화되지 않은 의사 코드이다. 이미 정렬된 값도 다시 비교하는 과정을 거치고 배열의 길이를 넘어서 까지 값을 비교하기 때문이다.

### 2-2. 최적화된 의사 코드

```javascript
function bubbleSort(arr) {
  const swap = (arr, idx1, idx2) =>
    ([arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]]);

  for (let i = arr.length; i > 0; i--) {
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) swap(arr, j, j + 1);
    }
  }

  return arr;
}
```

버블 정렬의 최적화된 의사 코드이다.

먼저 `swap` 함수를 만들어 재사용할 수 있게 만들 뿐 더러 가독성을 높여준다.

이미 비교가 끝난 것을 다시 비교하고 배열의 길이를 넘어서 까지 비교하는 하는 것을 없애고자 `i`가 `arr`의 길이부터 시작하여 1씩 감소하는 반복문으로 수정하였다. 이는 `j`가 몇 번 반복해야 하는지와 관련이 있다. `j`는 0부터 `i`에서 1을 뺀 숫자 만큼 반복을 하기 때문에 `i`가 1씩 감소하면 `j`도 계속해서 1씩 감소한다. 그러므로 이미 정렬된 값은 비교하지 않게 된다.

---

## 3. 버블 정렬의 문제점

`[1, 2, 4, 5, 7, 6]`와 같은 배열이 있다. 해당 배열을 버블 정렬을 사용하여 정렬을 하고자 하면 첫 번째 반복에서 `[1, 2, 4, 5, 6, 7]`의 결과를 반환한다. 이후 두 번째, 세 번째... 마지막 반복을 모두 마쳐 `[1, 2, 4, 5, 6, 7]`의 결과를 반환한다.

문제점이 보이는가? 첫 번째 반복의 결과와 마지막 최종 결과가 같다. 즉, 필요하지 않은 반복을 이어간다는 것이 버블 정렬의 문제점이다.

이를 개선하고자 하면 특정 반복에서 어떤 항목(값)이 교환하지 않았다면 정렬이 완료되었다는 것을 알려주는 코드가 필요하다.

아래는 이런 점을 보완한 코드이다.

```javascript
function bubbleSort(arr) {
  let noSwaps;

  const swap = (arr, idx1, idx2) => {
    [arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
    noSwaps = false;
  };

  for (let i = arr.length; i > 0; i--) {
    noSwaps = true;
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) swap(arr, j, j + 1);
    }
    if (noSwaps) break;
  }

  return arr;
}
```

`noSwaps` 변수르 만들어 첫 번째 `for 문`에서 항상 `true`로 만들고 두 번째 `for 문`에서 스왑이 발생하면 `false`로 만드는 것을 통해 문제점을 보완할 수 있다.

---

## 4. 버블 정렬의 시간 복잡도

일반적으로는 `O(n^2)`이다. 그나마 정렬이 거의다 되어 있고 아주 최적화된 버블 정렬을 사용한다면 선형 시간에 가깝다. 그렇다고 버블 정렬이 좋다는 것이 아니다. 버블 정렬은 다른 정렬과 비교할 때 큰 역할을 하므로 이정도만 알고 넘어가자. `O(n^2)`은 최악이다.

---

## 5. Conclusion

> `sort` 매서드만 사용했지 정렬에 대해서 깊게 생각해 본 적은 없다. 내부적으로 어떻게 동작하는지, 시간 복잡도는 어떻게 되는지 등 모른체 편하게 메서드만 사용하였다. 이번 기회에 여러 정렬에 대해 공부하여 효율적인 정렬을 사용하자.

---

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스 / 섹션 11: 버블 정렬

---

📅 2023-01-26