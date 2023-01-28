# 삽입 정렬(Insertion Sort)

## 1. 삽입 정렬이란?

> Builds up the sort by gradually creating a larger left half which is always sorted

삽입 정렬은 한 번에 하나를 취해서 올바른 위치에 삽입을 하는 방법이다. 이때 취한 값은 이미 정렬되어 있는 곳에서 올바른 위치를 찾아 삽입되게 된다. 계속해서 올바른 위치에 삽입을 하기 때문에 점진적으로 정렬이 완성이 된다.

즉, 이와 같이 정리할 수 있다. 자료 배열의 모든 요소를 앞에서부터 이미 정렬된 배열 부분과 비교 하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성한다.

이러한 삽입 정렬은 이전에 살펴본 버블 정렬과 선택 정렬과 비슷하지만 보다 유리한 지점이 있다. 정렬된 데이터에 새로운 데이터가 들어온다고 가정하자. 해당 데이터를 정렬을 해야하기 때문에 처음부터 다시 정렬을 시작한다. 버블 정렬과, 선택정렬은 처음부터 값을 비교하지만 삽입 정렬은 이미 정렬된 데이터는 무시할 수 있어 시간 복잡도 측면에서 큰 이점을 가진다.

### 1-1. 삽입 정렬의 간단한 예시

배열 [5, 3, 4, 1, 2]을 올림차순으로 정렬해보자.

- `[5, 3, 4, 1, 2]`: 앞 5와 비교하여 3을 올바른 위치에 삽입한다. -> `[3, 5, 4, 1, 2]`
- `[3, 5, 4, 1, 2]`: 앞 3, 5와 비교하여 4를 올바른 위치에 삽입한다. -> `[3, 4, 5, 1, 2]`
- `[3, 4, 5, 1, 2]`: 앞 3, 4, 5와 비교하여 1을 올바른 위치에 삽입한다. -> `[1, 3, 4, 5, 2]`
- `[1, 3, 4, 5, 2]`: 앞 1, 3, 4, 5와 비교하여 2를 올바른 위치에 삽입한다. -> `[1, 2, 3, 4, 5]`

---

## 2. 삽입 정렬의 의사 코드

```javascript
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let currentVal = arr[i];
    for (let j = i - 1; j >= 0 && arr[j] > currentVal; j--) {
      arr[j + 1] = arr[j];
      arr[j] = currentVal;
    }
  }
  return arr;
}
```

위는 숫자를 요소로 가지는 배열의 오름차순으로 삽입 정렬하는 의사 코드이다. 앞에서 살펴본 버블 정렬, 선택 정렬과 다르게 첫번째 `for... 문`은 `1` 부터 시작을 한다. 이유는 `0`번째 요소는 이미 정렬이 되어있다고 생각하기 때문이다.

첫번째 `for...문`에서 현재의 값을 `currentVal`에 저장을 하고 두번째 `for...문`에서 이전 값들과 비교하면서 `currentVal`의 새로운 위치를 찾아 삽입하고 있다. 눈여겨 봐야할 점은 두번째 `for...문`의 종료조건이다.

종료조건의 첫번째는 `j`가 음수일 때이다. 음수인 경우엔 비교할 대상이 없기 때문에 종료해야 한다. 종료조건 두번째는 `j`번째 요소가 `currentVal`보다 클 경우이다. 이 경우엔 두 요소의 위치를 바꾸지 않기 때문에 종료해야 한다.

---

## 3. 삽입 정렬의 시간 복잡도

삽입 정렬 또한 버블 정렬, 선택 정렬과 같다. 즉, 최악의 경우 `O(n^2)`의 시간 복잡도를 가지고 있다.

---

## 4. 버블 정렬, 선택 정렬, 삽입 정렬 비교

| Algorithm | 시간 복잡도(Best) | 시간 복잡도(Average) | 시간 복잡도(Worst) | 공간 복잡도 |
| --------- | ----------------- | -------------------- | ------------------ | ----------- |
| 버블 정렬 | O(n)              | O(n^2)               | O(n^2)             | O(1)        |
| 선택 정렬 | O(n^2)            | O(n^2)               | O(n^2)             | O(1)        |
| 삽입 정렬 | O(n)              | O(n^2)               | O(n^2)             | O(1)        |

---

## 5. Conclusion

> 가장 기본이 되는 정렬을 모두 살펴보았다. 버블 정렬, 선택 정렬, 삽입 정렬이 효율적인 정렬 알고리즘은 아니긴 하지만 특정 상황에서 유리하게 활용을 할 수 있다. 때문에 여러 예제를 통해 직접 고민하며 구현을 해 볼 필요가 있다고 생각한다. 틀린 답이더라도 직접 구현하고 시간을 얼마나 소요되는지 비교하면서 지식을 넓혀가고 싶다.  
> 정렬은 매우 기본적이며 중요하다. 면접에서도 많이 물어본다. 때문에 개념을 잘 짚고 넘어가는 것이 중요하다.

---

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스 / 섹션 13: 삽입 정렬  
[[알고리즘] 삽입 정렬(insertion sort)이란](https://gmlwjd9405.github.io/2018/05/06/algorithm-insertion-sort.html)

---

📅 2023-01-27