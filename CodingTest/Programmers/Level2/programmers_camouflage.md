# 위장

## 1. 개요

- 프로그래머스
- Lv.2
- 해시
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

---

## 2. 문제 설명

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '\_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

---

### 2-2. 문제 설명 - 입출력 예

| clothes                                                                                    | return |
| ------------------------------------------------------------------------------------------ | ------ |
| [["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]] | 5      |
| [["crow_mask", "face"], ["blue_sunglasses", "face"], ["smoky_makeup", "face"]]             | 3      |

---

### 2-3. 문제 설명 - 입출력 예 설명

예제 #1

headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

> 1. yellow_hat
> 2. blue_sunglasses
> 3. green_turban
> 4. yellow_hat + blue_sunglasses
> 5. green_turban + blue_sunglasses

예제 #2

face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

> 1. crow_mask
> 2. blue_sunglasses
> 3. smoky_makeup

---

## 3. 문제 풀이

```javascript
function solution(clothes) {
  const clothesMap = new Map();
  // 1) 같은 종류의 의상의 수 세기
  clothes.forEach((item) => {
    const [name, type] = item;
    clothesMap.set(type, (clothesMap.get(type) | 0) + 1);
  });

  // 2) 의상의 수를 요소로 가지는 배열 만들기
  const arr = [];
  for ([key, value] of clothesMap) {
    arr.push(value + 1);
  }

  // 3) 배열의 요소를 모두 곱한 값에 -1를 한 후 리턴하기
  return arr.reduce((a, b) => a * b) - 1;
}
```

---

### 1) 같은 종류의 의상의 수 세기

```javascript
clothes.forEach((item) => {
  const [name, type] = item;
  clothesMap.set(type, (clothesMap.get(type) | 0) + 1);
});
```

스파이가 가지고 있는 의상의 종류에 따른 의상의 수를 구해야 한다. `clothes`는 배열이며 요소 또한 배열이다.
요소의 0번째 인덱스는 의상의 이름이고 1번째 인덱스의 의상의 종류이다. 우리가 주목해야 할 부분은 의상의 이름이
아니라 의상의 종류이다.

그렇기 때문에 각각의 요소를 의미하는 `item`을 구조분해 할당하여 각각의 요소를 가져온다. 그 후
`Map.set()` 메서드를 사용하여 첫 번째 인자를 `type`, 두 번째 인자를 의상의 수를 구한 후 입력한다.

만약 `type`에 해당하는 데이터가 있다면 해당 데이터의 값에 1을 더하고 그렇지 않으면(처음 생성되는 데이터라면)
0에 1를 더한 값이 1이 저장된다.

---

### 2) 의상의 수를 요소로 가지는 배열 만들기

```javascript
const arr = [];
for ([key, value] of clothesMap) {
  arr.push(value + 1);
}
```

`arr` 배열에 의상의 종류에 따른 의상의 수를 저장한다. 그러기 위해서는 위에서 만든 `clothesMap` 맵 객체를
`for...of`을 사용하여 순회을 하면서 각 데이터의 값을 `arr` 배열에 넣어야 한다.

이때 `value`에 1을 더한 값을 배열에 넣는 이유는 어떤 종류의 의상을 입지 않는 경우도 생각해야 하기 때문이다.

---

### 3) 배열의 요소를 모두 곱한 값에 -1를 한 후 리턴하기

```javascript
return arr.reduce((a, b) => a * b) - 1;
```

`arr` 배열의 요소들을 모두 곱한 값에 1을 뺀 결과를 리턴한다.
여기서 `arr` 배열의 요소는 어떤 의상의 종류에서 고를 수 있는 의상의 경우의 수에 해당한다. 만약 의상의 종류가 모자이고 이 모자에는
의상이 총 3가지가 있다고 하면 모든 경우의 수는 모자를 쓰지 않는 경우의 수까지 포함하여 4가 된다.

모든 요소를 곱하는 것은 스파이가 입을 수 있는 의상 조합의 수이다. 마지막으로 -1를 한 까닭은 아무것도 입지 않는 경우를
뺀 것이다.

---

### 결과

![programmers_camouflage_result](/image/CodingTest/programmers_camouflage/programmers_camouflage_reuslt1.png)

---

## 4. 다른 사람 풀이

가장 많은 좋아요와 유사한 풀이가 많은 풀이를 가져왔다. 또한 나의 풀이를 포함하여 다른 풀이보다 깔끔한 느낌이 들어서 가져왔다.

```javascript
function solution(clothes) {
  return (
    // 2) 객체의 값을 요소로 가지는 배열 만들기
    Object.values(
      // 1)  같은 종류의 의상의 수 세기
      clothes.reduce((obj, t) => {
        obj[t[1]] = obj[t[1]] ? obj[t[1]] + 1 : 2;
        return obj;
      }, {})
      // 3) 배열의 요소를 모두 곱한 값에 -1를 한 후 리턴하기
    ).reduce((a, b) => a * (b + 1), 1) - 1
  );
}
```

---

### 1) 같은 종류의 의상의 수 세기

```javascript
clothes.reduce((obj, t) => {
  obj[t[1]] = obj[t[1]] ? obj[t[1]] + 1 : 2;
  return obj;
}, {});
```

`Array.reduce()`를 사용하여 같은 종류의 의상의 수를 세고 있다. 메서드의 초기값으로는 빈 객체 `{}`를 받고있다.
이후 순회를 통해 `clothes`의 요소의 1번째 인덱스에 해당하는 값이 `obj` 객체에 키로 존재한다면 해당 값에 1를 더하고
있지 않으면 2를 초기값으로 할당한다. 2를 초기값으로 할당하는 이유는 입지않는 경우의 수까지 세기 위함이다.

---

### 2) 객체의 값을 요소로 가지는 배열 만들기

```javascript
Object.values();
```

객체의 값을 요소로 가지는 배열을 만든다. 이때 만들어진 배열은 각 의상별 입을 수 있는 경우의 수를 의미한다.

---

### 3) 배열의 요소를 모두 곱한 값에 -1를 한 후 리턴하기

```javascript
_.reduce((a, b) => a * (b + 1), 1) - 1;
```

나의 풀이와 과정이 같으므로 생략한다.

---

### 결과

![programmers_camouflage_reuslt2](/image/CodingTest/programmers_camouflage/programmers_camouflage_reuslt2.png)

---

## 5. Conclusion

> 약 2주 전 풀었지만 실패했던 문제이다. 그 이유는 단순히 조합이라는 수학적 지식이 부족했기 때문이다. 각각의 경우의 수를
> 곱하기만 하면 쉽게 풀릴 문제를 너무 어렵고 복잡하게 생각을 하였다. 역시 공부를 하면 할 수록 앞으로
> 공부해야 할 내용에 대해 나름 머릿속으로 정리가 되어가는 듯 하다. 깊은 수학적 지식이 아니라 개발을 할 때
> 유용하게 쓰일 수학 내용도 공부를 해야겠다. 사실 책도 이미 주문을 했고 나중에 볼 강의도 알아보았다.
> 끝이 없어 보이지만 그만큼 심심하지도 않고 성장할 수 있는 기회가 많이 있다는 것이 즐겁다.  
> 아무튼 실제 코딩테스트에서도 위와 같이 알고리즘은 풀었지만 수학적 지식이 부족해서 틀린다면 매우 마음이 아플 듯 하다.
> 그래도 지금이라도 알게 되어서 다행이다... 다 중학교, 고등학교 때 배웠던 건데.... 10년이라는 세월이...
> 허송세월이구나...ㅠㅠㅠ반성...😭

---

📅 2022-09-20
