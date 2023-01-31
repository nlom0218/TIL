# 합병 정렬(Merge Sort)

## 1. 합병 정렬이란?

- It's a combination of two things - merging and sorting
- Exploits the fact that arrays of 0 or 1 element are always sorted
- Works by decomposing an array into smalller arrays of 0 or 1 elements, then building up a newly sorted array

합병 정렬은 `merge`, `sort`을 조합하여 정렬하는 방법이다. 길이가 0 또는 1인 배열은 항상 정렬이 되어있다는 사실을 기반으로 정렬하고자 하는 배열을 길이가 0 또는 1인 배열로 분해하고 분해 된 배열의 요소를 다시 합병하면서 정렬하는 것이다.

### 1-1. 합병 정렬의 간단한 예시

배열 [8, 3, 5, 4, 7, 6, 1, 2]를 오름차순으로 정렬해보자.

1. 먼저 해야 할 일은 길이가 0또는 1인 배열로 분해하는 것이다.
   - [8, 3, 5, 4] [7, 6, 1, 2]
   - [8, 3] [5, 4] [7, 6] [1, 2]
   - [8] [3] [5] [4] [7] [6] [1] [2]
2. 다음으론 합병하는 과정이다. 이때 합병하면서 정렬도 함께 해야 한다.
   - [3, 8] [4, 5] [6, 7] [1, 2]
   - [3, 4, 5, 8] [1, 2, 6, 7]
   - [1, 2, 3, 4, 5, 6, 7, 8]

---

## 2. 합병 정렬의 의사 코드

### 2-1. 합병하는 과정 의사 코드

합병 정렬의 전체 의사 코드를 작성하기 전, 정렬된 두 개의 배열을 합병하는 과정을 의사 코드를 작성하며 먼저 살펴보자.

```javascript
function merge(arr1, arr2) {
  let results = [];
  let i = 0;
  let j = 0;
  while (i < arr1.length && j < arr2.length) {
    if (arr2[j] > arr1[i]) {
      results.push(arr1[i]);
      i += 1;
    } else {
      results.push(arr2[j]);
      j += 1;
    }
  }

  while (i < arr1.length) {
    results.push(arr1[i]);
    i += 1;
  }
  while (j < arr2.length) {
    results.push(arr2[j]);
    j += 1;
  }

  return results;
}
```

첫번째 반복문에서는 `i`가 `arr1`의 길이보다 작고 `j`가 `arr2`의 길이보다 작을 때 까지 반복된다. `i`와 `j`를 포인터로 사용함으로써 값을 비교하고 `results` 배열에 어떤 요소가 추가될지 결정한다.

두번째, 세번재 반복문은 `arr1` 또는 `arr2` 에서 남은 요소를 `results` 배열에 추가하는 과정이다.

두번째, 세번째 반복문을 구조분해 할당과 `slice()` 메서드를 통해 아래와 같이 리펙터링을 하였다. 또는 해당 과정을 첫번째 반복문에 넣어 종료조건으로 사용할 수 있다.

```javascript
function merge(arr1, arr2) {
  let results = [];
  let i = 0;
  let j = 0;
  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) {
      results.push(arr1[i]);
      i += 1;
    } else {
      results.push(arr2[j]);
      j += 1;
    }
  }

  if (i < arr1.length) return [...results, ...arr1.slice(i)];
  if (j < arr2.length) return [...results, ...arr2.slice(j)];
}
```

### 2-2. 합병 정렬의 전체 의사 코드

```javascript
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  let mid = Math.floor(arr.length / 2);
  let left = mergeSort(arr.slice(0, mid));
  let right = mergeSort(arr.slice(mid));

  return merge(left, right);
```

`merge` 함수는 2-1. 에서 작성한 함수이다.

분할과 합병을 하는 재귀함수이다. 분할을 통해 배열이 단일 요소 또는 요소가 없는 상태까지 쪼개어 진다. 더 이상 쪼갤 수 없을 때, `left`와 `right`에 할당이 된다. 이렇게 `left`와 `right`에 배열이 할당이 되면 비로소 합병이 이루어진다. 합병이 이루어지면 합병 그리고 정렬된 배열이 만들어 지고 이는 또 다시 `left` 또는 `right`에 할당이 된다. 더 이상 합병을 할 수 없을 때 까지 반복한다.

---

## 3. 합병 정렬의 시간 복잡도

| 시간 복잡도(Best) | 시간 복잡도(Average) | 시간 복잡도(Worst) | 공간 복잡도 |
| ----------------- | -------------------- | ------------------ | ----------- |
| O(n log n)        | O(n log n)           | O(n log n)         | O(n)        |

배열을 절반 씩 분할을 하기 때문에 이는 `O(log n)`의 시간복잡도를 가진다. 분할 이후 다시 합병을 하는데 이때 `O(n)`의 시간복잡도를 가지므로 두 개의 시간 복잡도를 더해 `O(n log n)`이 된다.

---

## 4. Conclusion

> 진짜 멋진 재귀함수이다. 이전엔 한 번도 보지 못했던 알고리즘이여서 이해하는 것이 쉽지 않았다. 지금은 어떻게 동작을 하는지 이해는 했지만 이를 실제 문제에서 활용하기엔 많은 연습이 필요하다. 아무튼! 합병 정렬이라는 알고리즘을 배우면서 재귀 함수의 멋진 예시를 알게 되어 좋았다. 분할을 하는 동시에 다시 거슬러 올라와 합병을 하는... 정말 멋지다.

---

📅 2023-01-31
