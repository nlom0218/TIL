# 메뉴 리뉴얼

## 1. 개요

- 프로그래머스
- Lv.2
- 2021 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/72411)

---

## 2. 문제 설명

레스토랑을 운영하던 스카피는 코로나19로 인한 불경기를 극복하고자 메뉴를 새로 구성하려고 고민하고 있습니다.
기존에는 단품으로만 제공하던 메뉴를 조합해서 코스요리 형태로 재구성해서 새로운 메뉴를 제공하기로 결정했습니다. 어떤 단품메뉴들을 조합해서 코스요리 메뉴로 구성하면 좋을 지 고민하던 "스카피"는 이전에 각 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하기로 했습니다.
단, 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성하려고 합니다. 또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다.

예를 들어, 손님 6명이 주문한 단품메뉴들의 조합이 다음과 같다면,
(각 손님은 단품메뉴를 2개 이상 주문해야 하며, 각 단품메뉴는 A ~ Z의 알파벳 대문자로 표기합니다.)

| 손님 번호 | 주문한 단품메뉴 조합 |
| --------- | -------------------- |
| 1번 손님  | A, B, C, F, G        |
| 2번 손님  | A, C                 |
| 3번 손님  | C, D, E              |
| 4번 손님  | A, C, D, E           |
| 5번 손님  | B, C, F, G           |
| 6번 손님  | A, C, D, E, H        |

가장 많이 함께 주문된 단품메뉴 조합에 따라 "스카피"가 만들게 될 코스요리 메뉴 구성 후보는 다음과 같습니다.

| 코스 종류     | 메뉴 구성  | 설명                                                 |
| ------------- | ---------- | ---------------------------------------------------- |
| 요리 2개 코스 | A, C       | 1번, 2번, 4번, 6번 손님으로부터 총 4번 주문됐습니다. |
| 요리 3개 코스 | C, D, E    | 3번, 4번, 6번 손님으로부터 총 3번 주문됐습니다.      |
| 요리 4개 코스 | B, C, F, G | 1번, 5번 손님으로부터 총 2번 주문됐습니다.           |
| 요리 4개 코스 | A, C, D, E | 4번, 6번 손님으로부터 총 2번 주문됐습니다.           |

---

### 2-1. 문제 설명 - 문제

각 손님들이 주문한 단품메뉴들이 문자열 형식으로 담긴 배열 orders, "스카피"가 추가하고 싶어하는 코스요리를 구성하는 단품메뉴들의 갯수가 담긴 배열 course가 매개변수로 주어질 때, "스카피"가 새로 추가하게 될 코스요리의 메뉴 구성을 문자열 형태로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

---

### 2-2. 문제 설명 - 제한사항

- orders 배열의 크기는 2 이상 20 이하입니다.
- orders 배열의 각 원소는 크기가 2 이상 10 이하인 문자열입니다.
  - 각 문자열은 알파벳 대문자로만 이루어져 있습니다.
  - 각 문자열에는 같은 알파벳이 중복해서 들어있지 않습니다.
- course 배열의 크기는 1 이상 10 이하입니다.
  - course 배열의 각 원소는 2 이상 10 이하인 자연수가 오름차순으로 정렬되어 있습니다.
  - course 배열에는 같은 값이 중복해서 들어있지 않습니다.
- 정답은 각 코스요리 메뉴의 구성을 문자열 형식으로 배열에 담아 사전 순으로 오름차순 정렬해서 return 해주세요.
  - 배열의 각 원소에 저장된 문자열 또한 알파벳 오름차순으로 정렬되어야 합니다.
  - 만약 가장 많이 함께 주문된 메뉴 구성이 여러 개라면, 모두 배열에 담아 return 하면 됩니다.
  - orders와 course 매개변수는 return 하는 배열의 길이가 1 이상이 되도록 주어집니다.

---

### 2-3. 문제 설명 - 입출력 예

| orders                                            | course  | result                            |
| ------------------------------------------------- | ------- | --------------------------------- |
| ["ABCFG", "AC", "CDE", "ACDE", "BCFG", "ACDEH"]   | [2,3,4] | ["AC", "ACDE", "BCFG", "CDE"]     |
| ["ABCDE", "AB", "CD", "ADE", "XYZ", "XYZ", "ACD"] | [2,3,5] | ["ACD", "AD", "ADE", "CD", "XYZ"] |
| ["XYZ", "XWY", "WXA"]                             | [2,3,4] | ["WX", "XY"]                      |

---

### 2-4. 문제 설명 - 입출력 예에 대한 설명

입출력 예 #1
문제의 예시와 같습니다.

입출력 예 #2
AD가 세 번, CD가 세 번, ACD가 두 번, ADE가 두 번, XYZ 가 두 번 주문됐습니다.
요리 5개를 주문한 손님이 1명 있지만, 최소 2명 이상의 손님에게서 주문된 구성만 코스요리 후보에 들어가므로, 요리 5개로 구성된 코스요리는 새로 추가하지 않습니다.

입출력 예 #3
WX가 두 번, XY가 두 번 주문됐습니다.
3명의 손님 모두 단품메뉴를 3개씩 주문했지만, 최소 2명 이상의 손님에게서 주문된 구성만 코스요리 후보에 들어가므로, 요리 3개로 구성된 코스요리는 새로 추가하지 않습니다.
또, 단품메뉴를 4개 이상 주문한 손님은 없으므로, 요리 4개로 구성된 코스요리 또한 새로 추가하지 않습니다.

---

## 3. 문제 풀이

```js
// 1) 코스요리 메뉴 구성을 위한 함수
function getCombinations(array, size) {
  function p(t, i) {
    if (t.length === size) {
      result.push(t);
      return;
    }
    if (i + 1 > array.length) {
      return;
    }
    p(t.concat(array[i]), i + 1);
    p(t, i + 1);
  }

  var result = [];
  p([], 0);
  return result;
}

function solution(orders, course) {
  // 2) 각각의 손님이 주문한 메뉴의 구성을 보고 코스요리 메뉴 만들기
  const allCombi = [];
  orders.forEach((order) => {
    const allMenu = order.split("");
    course.forEach((num) => {
      const combiMenu = getCombinations(allMenu, num).map((i) => {
        return i.sort().join("");
      });
      combiMenu.forEach((menu) => {
        allCombi.push(menu);
      });
    });
  });

  // 3) 중복되는 코스요리 메뉴를 관리하기 위한 맵(Map)
  const map = new Map();
  allCombi.forEach((item, index) => {
    map.set(item, (map.get(item) || 0) + 1);
  });

  // 4) 맵(map)에 담긴 value와 mene를 각각 요소로 가지는 2차원 배열 만들기
  const combiMenu = [];
  map.forEach((value, key) => {
    combiMenu.push([key, value]);
  });

  const result = [];

  course.forEach((num) => {
    // 5) 추천 수 코스요리 메뉴의 길이가 num과 같은 요소만 추출하기
    const pickMenu = combiMenu.filter(([menu, pick]) => menu.length === num);

    // 6) 코스요리 메뉴가 없을 경우 해당 반복문 종료하기
    if (pickMenu.length === 0) return;

    // 7) 가장 높은 추천 수 찾기
    const bestPick = pickMenu.reduce((acc, [_, pick]) => {
      if (acc >= pick) {
        return acc;
      } else {
        return pick;
      }
    }, 0);

    // 8) 가장 높은 추천 수를 가진 코스요리 메뉴 찾기
    pickMenu.forEach(([menu, pick]) => {
      if (pick === 1) return;
      if (pick === bestPick) {
        result.push(menu);
      }
    });
  });

  // 9) 알파벳 순으로 정렬하기
  return result.sort();
}
```

---

### 1) 코스요리 메뉴 구성을 위한 함수

[소수 만들기](/CodingTest/Programmers/Level1/programmers_make-prime-number.md)에서 조합을 만들 때 사용했던 함수이다. 각각의 손님이 다수의 메뉴를 시키고 해당 메뉴를 겹치지 않는 여러 조합을 만들기 위해 이번 코딩 테스트에서도 사용하였다.

---

### 2) 각각의 손님이 주문한 메뉴의 구성을 보고 코스요리 메뉴 만들기

1. 먼저 다양한 조합의 코스요리 메뉴가 들어갈 배열을 만든다.

   ```js
   const allCombi = [];
   ```

2. 그 이후 `orders`배열을 `Array.forEach`메서드를 통해 반복문을 실행한다. `orders`배열에는 각각의 손님이 시킨 메뉴들을 문자열인 원소로 가지고 있다. 예를 들어 "ABCE"는 해당 손님이 메뉴A, 메뉴B, 메뉴C, 메뉴E를 주문한 것이다.

   ```js
   orders.forEach((order) => {});
   ```

3. 반복문 안에서 가장 먼저 할 것은 문자열을 배열로 만드는 것이다. 문자열을 배열로 만들기 위해서 `String.split()`메서드를 사용했다. 메서드의 인자로 들어가는 규칙에 따라 배열이 문자열이 쪼개서 생성된다. 빈 문자열을 인자로 넘기게 되면 문자 하나하나가 모두 분리된다.

   ```js
   const allMenu = order.split("");
   ```

   위의 결과 "ABCE"는 ["A", "B", "C", "E"]로 바뀌게 되어 `allMenu`변수에 저장된다.

4. `course`배열에는 몇 개의 요리로 코스요리 메뉴를 구성할 지를 알려주는 `number`값이 인지로 들어있다. 이를 `Array.forEach()`메서드를 사용해 반복문을 실행한다.

   ```js
   course.forEach((num) => {});
   ```

5. 위의 반복문 안에서는 `getCombinations()`함수가 실행된다. `allMenu`배열(손님이 주문한 모든 요리)과 `num`(요리의 수)을 인자로 받아 실행된 결과는 다음과 같을 것이다.(예시)

   - `allMenu`: ["A", "B", "C", "E"]
   - `num`: 3
   - `getCombinations(allMenu, num)`: [["A", "B", "C"],["A", "B", "E"],["A", "C", "E"],["B", "C", "E"]]

   이후 `Array.map()`메서드를 사용해 각 원소(배열)을 `Array.sort()`메서드로 알파벳 순으로 정렬하고 `Array.join("")`메세드를 통해 문자열로 바꾸었다. 그리고 이를 조합이 된 메뉴라는 뜻인 `combiMenu`라는 변수에 저장한다.

   최종 결과물은 ["ABC", "ABE", "ACE", "BCE"]가 된다.

   ```js
   const combiMenu = getCombination(allMenu, num).map((i) => i.sort().join(""));
   ```

6. 완성된 조합의 모든 코스요리 메뉴를 `allCombi` 배열에 저장한다.

   ```js
   combiMenu.forEach((menu) => {
     allCombi.push(menu);
   });
   ```

---

### 3) 중복되는 코스요리 메뉴를 관리하기 위한 맵(Map)

`allCombi`배열에는 각각의 손님들이 주문한 메뉴를 조합하여 만든 코스요리 메뉴가 저장되어 있기 때문에 중복되는 코스요리 메뉴가 존재한다. 즉, 중복된 요소가 존재한다. 얼마만큼 중복했는지가 해당 코스요리 메뉴가 최종 코스요리 메뉴가 될 수 있는지 없는지 판가름된다.

코스요리 메뉴가 얼마만큼 중복되는지 파악하기 위해 맵(Map)을 사용한다. 먼저 새로운 맵(Map)을 만든다.

```js
const map = new Map();
```

이후 `allCombi`배열을 `Array.forEach()`메서드를 사용하여 반복문들 돌면서 `Map.set(key, value)`메서드를 통해 map에 요소를 저장한다.

```js
allCombi.forEach((item, index) => {
  map.set(item, (map.get(item) || 0) + 1);
});
```

눈여겨 봐야할 것은 이미 `map`에 해당 코스요리 메뉴가 있으면 해당 코스요리 메뉴의 값(number)을 가져와 + 1를 하는 것이다. 이 과정을 통해 얼마만큼 코스요리 메뉴가 중복되는지 알 수 있다.

---

### 4) 맵(map)에 담긴 value와 mene를 각각 요소로 가지는 2차원 배열 만들기

얼마나 코스요리 메뉴가 중복되는지 이제 알 수 있다. 그 다음은 map을 반복문을 돌면서 [key, value]을 요소로 가지는 `combiMenu`배열을 만드는 것이다. `2) 각각의 손님이 주문한 메뉴의 구성을 보고 코스요리 메뉴 만들기`과정의 반복문 안에서 쓰인 변수명과 같다. 하지만 이만한 변수명이 없어 다시 쓴다.(스코프가 달라 큰 상관은 없긴하다.)

맵(Map)도 `Array.forEach()`메서드를 제공하기 때문에 이를 이용해 반복문을 사용했다.

참고로 아래의 코드에서 `value`는 코스요리 메뉴가 얼마만큼 중복되었는지를 나타나는 `number`값이고 `key`는 `string`으로 코스요리 메뉴의 이름이다.

```js
const combiMenu = [];
map.forEach((value, key) => {
  combiMenu.push([key, value]);
});
```

---

### 5) 추천 수 코스요리 메뉴의 길이가 num과 같은 요소만 추출하기

다시 `course`배열에 반복문을 사용했다. 그 이유는 해당 배열에 들어있는 값이 코스요리 메뉴를 구성하는 요리의 수이기 때문이다. 즉, 하나하나 돌면서 `course`배열의 각 요소인 숫자만큼 이루어진 코스요리 메뉴 중 가장 많이 중복되었던 코스요리 메뉴를 가져오기 위함이다.

앞으로의 `num`은 코스요리 메뉴를 구성하는 메뉴의 수를 의미한다.

반복문을 시작하고 가장 먼저 할 것인 `combiMenu`중 코스요리 메뉴의 길이가 `num`과 같은 것들을 가져오는 것이다. 이를위해 `Array.filter()`메서드를 사용했고 해당 값을 `pickNum`변수에 저장하였다.

```js
const pickMenu = combiMenu.filter(([menu, pick]) => menu.length === num);
```

---

### 6) 코스요리 메뉴가 없을 경우 해당 반복문 종료하기

만약 `pickNum`배열이 비어있을 경우에는 `num`값의 길이 만큼 구성된 코스요리 메뉴가 없다는 것이기 때문에 그대로 반복문을 종료하고 다음 반복문으로 넘어간다.

```js
if (pickMenu.length === 0) return;
```

---

### 7) 가장 높은 추천 수 찾기

`pickMenu`배열에는 같은 수 만큼 구성된 코스요리 메뉴의 정보가 들어있다. 그 중에서 가장 많이 중복되었던, 다른 말로는 가장 많이 추천을 받은 코스요리 메뉴를 찾아야 한다. 코스요리 메뉴를 찾기 전 가장 높은 추천 수가 얼마인지 `Array.reduce()`메서드를 통해 구한다.

```js
const bestPick = pickMenu.reduce((acc, [_, pick]) => {
  if (acc >= pick) return acc;
  else return pick;
}, 0);
```

---

### 8) 가장 높은 추천 수를 가진 코스요리 메뉴 찾기

위의 과정을 통해 가장 높은 추천 수를 알게 되었다. 이제 마지막으로 해당 추천 수와 `Array.forEach`메서드를 통해 같은 수의 구성을 가진 코스요리 메뉴 중 가장 많은 추천 수를 받은 코스요리 메뉴를 `result`배열에 저장하자.

주의해야 할 점은 어떤 코스요리 메뉴가 중복이 되지 않는다면 최종 코스요리 메뉴에 포함될 수 없다는 것이다. 아래의 코드에서는 `pick`이 1일 경우이다. 한 명만 해당 구성의 요리를 시킨 것이다. 이는 상품으로 가치가 없다.

```js
pickMenu.forEach(([menu, pick]) => {
  if (pick === 1) return;
  if (pick === bestPick) {
    result.push(menu);
  }
});
```

---

### 9) 알파벳 순으로 정렬하기

인기 있는 코스요리 메뉴가 담긴 `reuslt`배열을 제출하기 전 다시 알파벳 순으로 정렬한다.

```js
return result.sort();
```

---

### 결과

잘 통과된 모습을 볼 수 있다.👍

![programmers_menu_renewal](/image/CodingTest/programmers_menu_renewal/programmers_menu_renewal_result.png)

---

## 4. Refactoring

내가 풀이한 코드를 분석하다가 두 가지 정도를 리팩토링을 하고 싶었다. 최대한 간단하고 빠른 실행속도를 가지기 위해 고민을 하였고 아래의 두가지 내용에 대해 리팩토링을 하였다.

---

### 1) 필요없는 변수와 메서드 제거하기

첫 번째 리팩토링은 `2) 각각의 손님이 주문한 메뉴의 구성을 보고 코스요리 메뉴 만들기`에서 아래의 부분에 해당한다.

```js
course.forEach((num) => {
  const combiMenu = getCombinations(allMenu, num).map((i) => {
    return i.sort().join("");
  });
  combiMenu.forEach((menu) => {
    allCombi.push(menu);
  });
});
```

해당 코드는 `allCombi`배열에 메뉴를 넣는 코드이다. 여기서 굳이 `combiMenu`변수를 선언하지 않아도 될 거 같았다. 또한 `Array.map()`은 `allCombi`배열을 만들기 위해 사용했기 때문에 해당 메서드를 제거하고 `Array.forEach()`메서드로만 구현을 할 수 있을 거 같았다.

그리하여 위의 코드를 아래의 코드로 리팩토링하였다!! `i`를 `menu`로 바꾼건 덤🔥

```js
course.forEach((num) => {
  getCombinations(allMenu, num).forEach((menu) =>
    allCombi.push(menu.sort().join(""))
  );
});
```

---

### 2) 맵(Map)을 배열로 바꾸는 과정이 굳이 필요할까?

```js
// 4) 맵(map)에 담긴 value와 mene를 각각 요소로 가지는 2차원 배열 만들기
const combiMenu = [];
map.forEach((value, key) => {
  combiMenu.push([key, value]);
});
```

위의 코드가 `map`를 2차원 배열로 만드는 과정이다. 이 과정이 굳이 필요할까? 라는 의문이 들었다. 해당 과정만 없어도 실행속도는 조금더 빨라지지 않을까?

그래서 위의 코드를 지우고 대신 아래와 같이 수정하였다.

1. 수정 전

   ```js
   // 5) 추천 수 코스요리 메뉴의 길이가 num과 같은 요소만 추출하기
   const pickMenu = combiMenu.filter(([menu, pick]) => menu.length === num);
   ```

2. 수정 후

   ```js
   const pickMenu = [];
   map.forEach((pick, menu) => {
     if (menu.length === num) {
       pickMenu.push([menu, pick]);
       map.delete(menu);
     }
   });
   ```

위의 내용은 `pickMenu`배열을 선언하고 `map`을 `Array.forEach()`메서드를 사용해 반복문을 돌며 `pickMenu`에 조건에 맞는 값을 저장하고 있는 과정을 나타낸 코드이다. 기존과 같은 같의 `pickMenu`배열이 생길 것이다.

```js
pickMenu.push([menu, pick]);
map.delete(menu);
```

`pickMenu`배열에 값을 추가하고 `map`에서 해당 `menu(key)`를 지우는 이유는 다음 반복문을 돌 때 불필요한 요소에 대해서는 돌지 않기 위함이다.

---

### 결과

실행 속도가 빨리진 것을 확인할 수 있다🚀

![programmers_menu_renewal_result 2](/image/CodingTest/programmers_menu_renewal/programmers_menu_renewal_result2.png)

---

## 5. 다른 사람의 풀이

가장 많은 좋아요를 받은 풀이를 가져왔다.

```js
function solution(orders, course) {
  const ordered = {};
  const candidates = {};
  const maxNum = Array(10 + 1).fill(0);
  const createSet = (arr, start, len, foods) => {
    if (len === 0) {
      ordered[foods] = (ordered[foods] || 0) + 1;
      if (ordered[foods] > 1) candidates[foods] = ordered[foods];
      maxNum[foods.length] = Math.max(maxNum[foods.length], ordered[foods]);
      return;
    }

    for (let i = start; i < arr.length; i++) {
      createSet(arr, i + 1, len - 1, foods + arr[i]);
    }
  };

  orders.forEach((od) => {
    // arr.sort는 기본적으로 사전식 배열
    const sorted = od.split("").sort();
    // 주문한 음식을 사전순으로 배열해서 문자열을 만든다.
    // course에 있는 길이만 만들면 된다.
    course.forEach((len) => {
      createSet(sorted, 0, len, "");
    });
  });

  const launched = Object.keys(candidates).filter(
    (food) => maxNum[food.length] === candidates[food]
  );

  return launched.sort();
}
```

해당 풀이가 대단하다고 느끼는 이유는 재귀함수의 활용 때문이다. 나도 물론 재귀함수를 통해 조합배열을 만들었다. 하지만 이는 스택오버플로워의 도움을 받은 것고 이를 단순히 사용만 했던 것이다. 하지만 위의 코드에서는 `createSet()`함수 안에서 반복적으로 자기자신을 호출하고 있기 때문에 문제의 특징에 맞게 코드를 작성한 것으로 보인다. 문제를 보는 눈과 코드를 구성하고 작성하는 능력이 정말 대단하다. 코딩 테스트 공부와 코딩, 자바스크립트 공부에 동기부여를 얻고 가자!

---

## 6. Conclusion

> 오늘은 처음으로 Level2의 문제를 풀어봤다. 하지만 중간에 중요한 일이 생겨 흐름이 끊기긴 했지만 혼자 고민하여 끝까지 문제를 풀고 정답까지 맞추게 되어 재밌는 경험이었다. 물론... 아직 시간을 너무 많이 사용한다는 것이 흠이다🥹  
> 어제 배운 맵(Map)도 적용하여 풀어보니 뿌듯함도 든다. 다른 사람의 풀이에서도 느꼈던 내용 처럼 아는만큼 보이는 코딩 테스트, 코드인 듯 하다. 그나저나 정답을 맞추는 것은 뒤로하고 시간 단축은 어찌해야 할까ㅠㅠ 지금은 경험이 많이 있지 않기 때문이라고 생각하지만 시간 문제가 해결되지 않으면 근본적인 문제를 고민해봐야지..

---

[👆](#메뉴-리뉴얼)

📅 2022-08-09
