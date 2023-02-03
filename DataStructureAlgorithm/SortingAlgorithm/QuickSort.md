# 퀵 정렬(Quick Sort)

## 1. 퀵 정렬이란?

- Like merge sort, exploits the fact that arrays of 0 or 1 element are always sorted
- Works by selecting one element(called ths "pivot") and finding the index where the pivot should end up in the sorted array
- Once the pivot is positioned apporpriately, quick sort can be applied on either side of the pivot

퀵 정렬은 `pivot`이라는 특정 요소를 선택하여 `pivot`을 기준으로 왼쪽과 오른쪽을 나눈다. 만약 오름차순일 경우, 왼쪽엔 `pivot`보다 작은 요소를 오른쪽엔 `pivot`보다 큰 요소를 왼쪽에 옮긴다. 이렇게 된다면 `pivot`으로 선택된 요소는 자신의 위치를 찾게된다. 이후엔 왼쪽과 오른쪽을 각각 새로운 `pivot`을 정하고 배열이 모두 정렬될 때 까지 같은 작업을 반복하게 된다.

### 1-1. 퀵 정렬의 간단한 예시

[5, 2, 1, 8, 4, 7, 6, 3] 배열을 오름차순으로 정럴해보자. 처음으로 선택된 `pivot`은 `0`번째 인덱스인 `5`이다.

1. [2, 1, 4, 3, 5, 8, 7, 6] - `5`는 자신의 위치를 잘 찾아갔다. 위치를 잘 찾아갔으니 이번엔 `5`를 기준으로 왼쪽에 있는 요소들 부터 다시 정렬을 해보자. 이때의 `pivot`은 `2`가 된다.
2. [1, 2, 4, 3, 5, 8, 7, 6] - 왼쪽 `pivot`은 `2`이다. 2보다 작은 1은 왼쪽에 2보다 큰 4, 3은 오른쪽으로 이동한다. 이때 `2`는 자신의 위치를 잘 찾아갔다. 이를 계속 반복하면 다음 과정과 같다.
3. [1, 2, 3, 4, 5, 8, 7, 6] - `4`가 자신의 위치를 찾아갔고 홀로 남은 3은 항상 정렬된다는 사실 아래에 더 이상 움직이지 않아도 된다.
4. [1, 2, 3, 4, 5, 7, 6, 8] - `8`이 자신의 위치를 찾아갔다.
5. [1, 2, 3, 4, 5, 6, 7, 8] - `7`이 자신의 위치를 찾아갔고 홀로 남은 6은 그대로 자신의 위치가 정해졌다.

---

## 2. 퀵 정렬의 의사 코드

### 2-1. pivot helper 의사 코드

`pivot`으로 선택된 요소의 위치를 반환하는 `pivot helper` 의사 코드를 작성해보자. 해당 과정을 통해 기존 배열의 위치도 바뀌게 된다. 즉, `pivot`으로 선택된 요소가 자신의 자리를 찾아간 배열로 바뀐다.

```javascript
function pivot(arr, start = 0, end = arr.length - 1) {
  let pivot = arr[start];
  let swapIdx = start;

  for (let i = start + 1; i <= end; i++) {
    if (pivot < arr[i]) continue;
    swapIdx += 1;
    [arr[swapIdx], arr[i]] = [arr[i], arr[swapIdx]];
  }

  [arr[swapIdx], arr[start]] = [arr[start], arr[swapIdx]];
  return swapIdx;
}
```

### 2-2. 퀵 정렬의 전체 의사 코드

```javascript
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (left >= right) return;
  let pivotIndex = pivot(arr, left, right);
  quickSort(arr, left, pivotIndex - 1);
  quickSort(arr, pivotIndex + 1, right);

  return arr;
}
```

퀵 정렬의 의사 코드는 기본적으로 재귀적으로 동작하는 특징을 가지고 있다. 이는 합병 정렬과 같은 점이다. 그럼 코드는 어떻게 동작하는지 살펴보자.

먼저 `pivot` 함수를 통해 특정 요소가 위치하는 인덱스를 `pivotIndex`에 할당한다. 해당 값을 통해 두개의 `quickSort` 함수를 호출을 하는데, 이는 `pivotIndex`를 기준으로 왼쪽과 오른쪽의 요소들을 정렬해야 하기 때문이다.

그렇다면 종료조건은 얻허게 될까? 그것은 바로 `left`와 `right`를 비교하여 `left`가 같거나 더 크면 종료된다.

---

## 3. 퀵 정렬의 시간 복잡도

| 시간 복잡도(Best) | 시간 복잡도(Average) | 시간 복잡도(Worst) | 공간 복잡도 |
| ----------------- | -------------------- | ------------------ | ----------- |
| O(n log n)        | O(n log n)           | O(n^2)             | O(n log n)  |

최상의 케이스인 경우엔 `O(n log n)`의 시간 복잡도를 가진다. 이는 정말 기가 막히게 `pivot`이 배열의 중간 지점에 위치하여 계속해서 반으로 줄어드는 상황에 해당된다.

하지만 최악의 경우엔 `O(n^2)`의 시간 복잡도를 가진다. 해당 경우는 어떤 상황일까? 바로 이미 정렬된 상황이다. 즉, `pviot`을 구하지만 배열을 왼쪽과 오른쪽으로 나누어지지 않는다.

---

## 4. Conclusion

> 퀵 정렬에 대해 살펴보았다. 합병 정렬과 같이 재귀 함수를 작성하기 위해 많은 시간을 쏟았고 스스로 구현을 하지 못해 아쉬운 점이 있지만, 완성된 코드를 보고 많은 것을 배웠다. 재귀 함수를 사용하는 것 뿐 아니라, 퀵 정렬이라는 알고리즘의 동작 원리에 대해 고민할 수 있어서 많은 도움이 되었다. 이런 기가막힌 방법은 어떻게 생각해 내는지 그저 감탄만 나올 뿐 이다. 나도 많은 경험을 쌓고 꾸준히 공부를 하다 보면 언제가는 도달할 수 있는 경지겠지?

---

📅 2023-02-03
