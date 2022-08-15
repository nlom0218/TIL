# 뉴스 클러스터링

## 1. 개요

- 프로그래머스
- Lv.2
- 2018 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/17677)

---

## 2. 문제 설명

여러 언론사에서 쏟아지는 뉴스, 특히 속보성 뉴스를 보면 비슷비슷한 제목의 기사가 많아 정작 필요한 기사를 찾기가 어렵다. Daum 뉴스의 개발 업무를 맡게 된 신입사원 튜브는 사용자들이 편리하게 다양한 뉴스를 찾아볼 수 있도록 문제점을 개선하는 업무를 맡게 되었다.

개발의 방향을 잡기 위해 튜브는 우선 최근 화제가 되고 있는 "카카오 신입 개발자 공채" 관련 기사를 검색해보았다.

- 카카오 첫 공채..'블라인드' 방식 채용
- 카카오, 합병 후 첫 공채.. 블라인드 전형으로 개발자 채용
- 카카오, 블라인드 전형으로 신입 개발자 공채
- 카카오 공채, 신입 개발자 코딩 능력만 본다
- 카카오, 신입 공채.. "코딩 실력만 본다"
- 카카오 "코딩 능력만으로 2018 신입 개발자 뽑는다"

기사의 제목을 기준으로 "블라인드 전형"에 주목하는 기사와 "코딩 테스트"에 주목하는 기사로 나뉘는 걸 발견했다. 튜브는 이들을 각각 묶어서 보여주면 카카오 공채 관련 기사를 찾아보는 사용자에게 유용할 듯싶었다.

유사한 기사를 묶는 기준을 정하기 위해서 논문과 자료를 조사하던 튜브는 "자카드 유사도"라는 방법을 찾아냈다.

자카드 유사도는 집합 간의 유사도를 검사하는 여러 방법 중의 하나로 알려져 있다. 두 집합 A, B 사이의 자카드 유사도 J(A, B)는 두 집합의 교집합 크기를 두 집합의 합집합 크기로 나눈 값으로 정의된다.

예를 들어 집합 A = {1, 2, 3}, 집합 B = {2, 3, 4}라고 할 때, 교집합 A ∩ B = {2, 3}, 합집합 A ∪ B = {1, 2, 3, 4}이 되므로, 집합 A, B 사이의 자카드 유사도 J(A, B) = 2/4 = 0.5가 된다. 집합 A와 집합 B가 모두 공집합일 경우에는 나눗셈이 정의되지 않으니 따로 J(A, B) = 1로 정의한다.

자카드 유사도는 원소의 중복을 허용하는 다중집합에 대해서 확장할 수 있다. 다중집합 A는 원소 "1"을 3개 가지고 있고, 다중집합 B는 원소 "1"을 5개 가지고 있다고 하자. 이 다중집합의 교집합 A ∩ B는 원소 "1"을 min(3, 5)인 3개, 합집합 A ∪ B는 원소 "1"을 max(3, 5)인 5개 가지게 된다. 다중집합 A = {1, 1, 2, 2, 3}, 다중집합 B = {1, 2, 2, 4, 5}라고 하면, 교집합 A ∩ B = {1, 2, 2}, 합집합 A ∪ B = {1, 1, 2, 2, 3, 4, 5}가 되므로, 자카드 유사도 J(A, B) = 3/7, 약 0.42가 된다.

이를 이용하여 문자열 사이의 유사도를 계산하는데 이용할 수 있다. 문자열 "FRANCE"와 "FRENCH"가 주어졌을 때, 이를 두 글자씩 끊어서 다중집합을 만들 수 있다. 각각 {FR, RA, AN, NC, CE}, {FR, RE, EN, NC, CH}가 되며, 교집합은 {FR, NC}, 합집합은 {FR, RA, AN, NC, CE, RE, EN, CH}가 되므로, 두 문자열 사이의 자카드 유사도 J("FRANCE", "FRENCH") = 2/8 = 0.25가 된다.

---

### 2-1. 문제 설명 - 입력 형식

- 입력으로는 str1과 str2의 두 문자열이 들어온다. 각 문자열의 길이는 2 이상, 1,000 이하이다.
- 입력으로 들어온 문자열은 두 글자씩 끊어서 다중집합의 원소로 만든다. 이때 영문자로 된 글자 쌍만 유효하고, 기타 공백이나 숫자, 특수 문자가 들어있는 경우는 그 글자 쌍을 버린다. 예를 들어 "ab+"가 입력으로 들어오면, "ab"만 다중집합의 원소로 삼고, "b+"는 버린다.
- 다중집합 원소 사이를 비교할 때, 대문자와 소문자의 차이는 무시한다. "AB"와 "Ab", "ab"는 같은 원소로 취급한다.

---

### 2-2. 문제 설명 - 출력 형식

입력으로 들어온 두 문자열의 자카드 유사도를 출력한다. 유사도 값은 0에서 1 사이의 실수이므로, 이를 다루기 쉽도록 65536을 곱한 후에 소수점 아래를 버리고 정수부만 출력한다.

---

### 2-3. 문제 설명 - 예제 입출력

| str1      | str2        | answer |
| --------- | ----------- | ------ |
| FRANCE    | french      | 16384  |
| handshake | shake hands | 65536  |
| aa1+aa2   | AAAA12      | 43690  |
| E=M\*C^2  | e=m\*c^2    | 65536  |

---

## 3. 문제 풀이

```js
function solution(str1, str2) {
  // 1) 문자열을 모두 소문자로 바꾼 후 각각의 문자를 요소로 하는 배열 만들기
  arrStr1 = [...str1.toLowerCase()];
  arrStr2 = [...str2.toLowerCase()];

  const combiStr1 = [];
  for (let i = 0; i < arrStr1.length - 1; i++) {
    // 2) 영문자로 된 글자 쌍이 아닐 경우엔 다중 집합의 원소로 만들기 않기
    const regex = /[^a-z]/;
    if (regex.test(arrStr1[i]) || regex.test(arrStr1[i + 1])) continue;
    // 3) 영문자로 된 글자 쌍일 경우에만 다중 집합의 원소로 만들기
    combiStr1.push(arrStr1[i] + arrStr1[i + 1]);
  }
  const combiStr2 = [];
  for (let i = 0; i < arrStr2.length - 1; i++) {
    const regex = /[^a-z]/;
    if (regex.test(arrStr2[i]) || regex.test(arrStr2[i + 1])) continue;
    combiStr2.push(arrStr2[i] + arrStr2[i + 1]);
  }

  const newCombiStr1 = [];
  // 4) 두 배열의 교집합 만들기
  const intersection = combiStr1.filter((item) => {
    if (combiStr2.includes(item)) {
      const index = combiStr2.indexOf(item);
      // 4-1) combiStr2 배열에만 있는 요소만 남기기(차집합)
      combiStr2.splice(index, 1);
      return true;
    }
    // 4-2) combiStr1 배열에만 있는 요소 만들기(차집합)
    newCombiStr1.push(item);
    return false;
  });
  // 5) 두 배열의 합집합 만들기
  const union = [...intersection, ...newCombiStr1, ...combiStr2];

  // 6) 교집합과 합집합이 모두 0인 경우와 교집합만 0인 경우
  if (intersection.length === 0) {
    if (union.length === 0) return 65536;
    return 0;
  }

  // 7) 두 배열의 자카드 유사도 구하기
  return Math.floor((intersection.length / union.length) * 65536);
}
```

---

### 1) 문자열을 모두 소문자로 바꾼 후 각각의 문자를 요소로 하는 배열 만들기

```js
arrStr1 = [...str1.toLowerCase()];
arrStr2 = [...str2.toLowerCase()];
```

다중집합 원소 사이를 비교할 때, 대문자와 소문자의 차이는 무시한다. 그렇기 대문에 `String.toLowerCase()`메서드를 사용하여 입력으로 들어온 문자열 `str1`, 문자열 `str2`의 알파벳을 모두 소문자로 바꾼다.

그 후 `스프레드 연산자(Spread Operator)`를 이용하여 문자열을 배열로 바꾼다. 문자열을 구성하는 문자들 하나하나가 요소가 된다.

문자열을 배열로 바꾸는 방법은 `스프레드 연산자`뿐만 아니라 `Array.from()`메서드, `String.split()`메서드도 있다.

---

### 2) 영문자로 된 글자 쌍이 아닐 경우엔 다중 집합의 원소로 만들기 않기

```js
const regex = /[^a-z]/;
if (regex.test(arrStr1[i]) || regex.test(arrStr1[i + 1])) continue;
```

정규식인 `/[^a-z]/`는 알파벳을 제외한 문자를 뜻한다. 만약 `arrStr1`의 특정 요소와 그 다음 요소에 대해 정규식 테스트 진행을 하게 되었을 때, 두 문자 중 하나라도 알파벳을 제외한 문자라면 다음 반복문으로 진행한다.

---

### 3) 영문자로 된 글자 쌍일 경우에만 다중 집합의 원소로 만들기

```js
combiStr1.push(arrStr1[i] + arrStr1[i + 1]);
```

만약 `arrStr1`의 특정 요소와 그 다음 요소가 모두 알파벳이라면 두 문자를 더한 문자열을 `Array.push()` 메서드를 사용하여 `combiStr1` 배열에 추가한다.

이때 `combiStr1` 배열은 `str1` 문자열에서 서로 이웃한 문자가 요소로 들어간 배열이다.

`combiStr2` 배열도 같은 방법으로 만들어진다.

---

### 4) 두 배열의 교집합 만들기

```js
const intersection = combiStr1.filter((item) => {
  if (combiStr2.includes(item)) {
    const index = combiStr2.indexOf(item);
    // 4-1) combiStr2 배열에만 있는 요소만 남기기(차집합)
    combiStr2.splice(index, 1);
    return true;
  }
  // 4-2) combiStr1 배열에만 있는 요소 만들기(차집합)
  newCombiStr1.push(item);
  return false;
});
```

`combiStr1` 배열과 `combiStr2` 배열의 교집합을 만드는 과정이다. 해당 과정에서 두 개의 차집합도 만들어진다.

1. 두 배열 모두 같은 특정 요소가 있는 경우

   ```js
   const index = combiStr2.indexOf(item);
   combiStr2.splice(index, 1);
   return true;
   ```

   - Array.indexOf: `combiStr2` 배열에서 해당 요소의 인덱스를 찾는다.
   - Array.splice(): 해당 요소의 인덱스부터 1개의 요소를 제거한다.
   - 위의 과정을 통해 `combiStr2` 배열의 요소는 삭제가 되면 결국엔 `combiStr1` 배열에는 없고 `combiStr2` 배열에만 있는 요소만 남게된다.(차집합)
   - `true`를 리턴하기 때문에 해당 요소는 `intersection`에 추가된다.

   ***

2. `combiStr1` 배열에만 특정 요소가 있는 경우
   ```js
   newCombiStr1.push(item);
   return false;
   ```
   - Array.push(): `newCombiStr1` 배열은 `combiStr1` 배열에만 있는 요소를 가지게 된다.(차집합)
   - `false`를 리턴하기 때문에 해당 요소는 `intersection`에 추가되지 않는다.

---

### 5) 두 배열의 합집합 만들기

```js
const union = [...intersection, ...newCombiStr1, ...combiStr2];
```

`4) 두 배열의 교집합 만들기` 과정을 통해 만들어진 교집합과 두 차집합을 이용하여 합집합을 만든다. 이때 `스프레드 연산자`를 사용했지만 `Array.concat()` 메서드를 사용하여 만들 수도 있다.

```js
const union = intersection.concat(newCombiStr1, combiStr2);
```

---

### 6) 교집합과 합집합이 모두 0인 경우와 교집합만 0인 경우

```js
if (intersection.length === 0) {
  if (union.length === 0) return 65536;
  return 0;
}
```

만약 교집합과 합집합이 모두 0인 경우에는 나눗셈을 할 수 없으니 `65536`을 리턴한다.(문제의 조건에 명시되어 있다.)

또한 교집합만 0이라면 나눗셈의 결과는 무조건 0이기 때문에 `0`을 리턴한다.

---

### 7) 두 배열의 자카드 유사도 구하기

```js
return Math.floor((intersection.length / union.length) * 65536);
```

위의 조건문에 해당되지 않는 내용이라면 위와 같은 과정을 통해 자카드 유사도를 구한다.

문제 설명에서 나온 자카드 유사도의 설명은 아래와 같다.

> 자카드 유사도는 집합 간의 유사도를 검사하는 여러 방법 중의 하나로 알려져 있다. 두 집합 A, B 사이의 자카드 유사도 J(A, B)는 두 집합의 교집합 크기를 두 집합의 합집합 크기로 나눈 값으로 정의된다.

원래의 자카드 유사도는 0~1사이의 실수이다. 하지만 아래에 보이는 것과 같이 문제에서 원하는 답은 실제의 자카드 유사도가 아니라 해당 유사도에 `65536`을 곱한 후의 정수부분이다. `Math.floor()` 메서드는 소수점 아래의 부분을 버리는 역할을 한다.

> 유사도 값은 0에서 1 사이의 실수이므로, 이를 다루기 쉽도록 65536을 곱한 후에 소수점 아래를 버리고 정수부만 출력한다.

---

### 결과

![programmers_news_clustering_result1](/image/CodingTest/programmers_news_clustering/programmers_news_clustering_result1.png)

---

## 4. Refactoring

### 1) 중복된 부분을 하나의 함수로 만들어 관리하기

```js
arrStr1 = [...str1.toLowerCase()];
arrStr2 = [...str2.toLowerCase()];

const combiStr1 = [];
for (let i = 0; i < arrStr1.length - 1; i++) {
  const regex = /[^a-z]/;
  if (regex.test(arrStr1[i]) || regex.test(arrStr1[i + 1])) continue;
  combiStr1.push(arrStr1[i] + arrStr1[i + 1]);
}

const combiStr2 = [];
for (let i = 0; i < arrStr2.length - 1; i++) {
  const regex = /[^a-z]/;
  if (regex.test(arrStr2[i]) || regex.test(arrStr2[i + 1])) continue;
  combiStr2.push(arrStr2[i] + arrStr2[i + 1]);
}
```

위의 코드는 `str1` 문자열과 `str2` 문자열을 바탕으로 다중집합 배열을 만드는 과정이다. `str1`과 `str2`만 다를 뿐 모든 내용이 같다. 그래서 이를 함수로 만들어 관리하고자 한다. 함수는 아래와 같이 만들었다.

```js
const makeCombiStr = (str) => {
  const arr = [...str.toLowerCase()];

  const combiStr = [];
  for (let i = 0; i < arr.length - 1; i++) {
    const regex = /[^a-z]/;
    if (regex.test(arr[i]) || regex.test(arr[i + 1])) continue;
    combiStr.push(arr[i] + arr[i + 1]);
  }

  return combiStr;
};
```

그리고 다중집합 배열은 아래와 같은 방법으로 만든다.

```js
const combiStr1 = makeCombiStr(str1);
const combiStr2 = makeCombiStr(str2);
```

---

### 2) 교집합(intersection)이 0인 경우를 굳이 나누어야 할까?

교집합이 0인 경우 합집합이 어떤 값이더라도 자카드 유사도는 0이 나온다. 처음에는 해당 경우도 `if`문으로 통해 나누어서 리턴을 했지만 굳이 그럴 필요가 없다고 생각한다.

그래서 아래와 같이 리턴하는 부분을 수정하였다.

```js
if (intersection.length === 0 && union.length === 0) return 65536;
return Math.floor((intersection.length / union.length) * 65536);
```

---

### 결과

실행 속도가 매우 빨리진 것을 확인할 수 있다👍 👍

![programmers_news_clustering_result2](/image/CodingTest/programmers_news_clustering/programmers_news_clustering_result2.png)

---

## 5. 다른 사람 풀이

가장 많은 좋아요를 받은 풀이이다.

```js
function solution(str1, str2) {
  function explode(text) {
    const result = [];
    for (let i = 0; i < text.length - 1; i++) {
      // 1) String.substr() 메서드를 사용하여 문자열 자르기
      const node = text.substr(i, 2);
      // 2) 정규식과 매치되는지 확인하기
      if (node.match(/[A-Za-z]{2}/)) {
        result.push(node.toLowerCase());
      }
    }
    return result;
  }

  const arr1 = explode(str1);
  const arr2 = explode(str2);
  // 3) arr1 배열과 arr2 배열 셋(Set)을 이용하여 합치기
  const set = new Set([...arr1, ...arr2]);
  let union = 0;
  let intersection = 0;

  set.forEach((item) => {
    // 4) 합집합과 교집합의 수 구하기
    const has1 = arr1.filter((x) => x === item).length;
    const has2 = arr2.filter((x) => x === item).length;
    union += Math.max(has1, has2);
    intersection += Math.min(has1, has2);
  });
  return union === 0 ? 65536 : Math.floor((intersection / union) * 65536);
}
```

---

### 1) String.substr() 메서드를 사용하여 문자열 자르기

```js
const node = text.substr(i, 2);
```

나는 배열로 바꾼 다음 요소를 더해 길이가 2인 문자열을 만들었다. 하지만 여기서는 굳이 배열로 만들지 않고 문자열 관련 메서드를 사용하여 길이가 2인 문자열을 만든다.

`String.substr()` 메서드는 문자열에서 특정 위치에서 시작하여 특정 문자 수 만큼의 문자들을 반환한다. `MDN`에서는 해당 메서드에 대해 아래와 같은 경고를 주고있다.

> 프로그래머는 새로운 ECMAScript 코드를 작성할 때 본 부록이 포함한 기능을 사용하거나 존재함을 가정해선 안됩니다.

즉, 해당 메서드가 아닌 다른 메서드로 위의 코드를 대체하고자 한다.

1. String.substring(): `string` 객체의 시작 인덱스로 부터 종료 인덱스 전 까지 문자열의 부분 문자열을 반환한다.

   ```js
   const node = text.substring(i, i + 2);
   ```

2. String.slice(): 문자열의 일부를 추출하면서 새로운 문자열을 반환한다.

   ```js
   const node = text.slice(i, i + 2);
   ```

---

### 2) 정규식과 매치되는지 확인하기

```js
if (node.match(/[A-Za-z]{2}/)) {
  result.push(node.toLowerCase());
}
```

1. String.match(reqexp): 문자열이 정규식과 매치되는 부분을 검색한다.
2. 정규식 `/[A-Za-z]{2}/`
   - [A-Za-z]: 각각의 문자가 알파벳 대문자, 소문자인지 검사
   - {2}: 앞의 패턴을 가지는 문자가 2개인지 검사
3. String.toLowerCase(): 정규식 테스트에 통과한 문자열인 경우 소문자로 바꾼다.
4. Array.push(): `result` 배열에 추가한다.

---

### 3) arr1 배열과 arr2 배열 셋(Set)을 이용하여 합치기

```js
const set = new Set([...arr1, ...arr2]);
```

`arr1` 배열과 `arr2` 배열을 합친다. 다만 중복되는 요소는 제거한다.(하나만 남긴다.)

해당 과정을 통해 `set` 객체에는 아래의 값을 포함한다.

1. `arr1` 배열에만 있는 요소
2. `arr2` 배열에만 있는 요소
3. `arr1` 배열, `arr2` 배열 모두에 있는 요소

---

### 4) 합집합과 교집합의 수 구하기

```js
const has1 = arr1.filter((x) => x === item).length;
const has2 = arr2.filter((x) => x === item).length;
union += Math.max(has1, has2);
intersection += Math.min(has1, has2);
```

`Math.max()` 메서드를 통해 합집합의 수를 구하고 `Math.min()` 메서드를 통해 교집합의 수를 구한다.

---

다른 사람 풀이를 공부하면서 `셋(Set)`의 활용과 `Math.max()` 메서드와 `Math.min()` 메서드를 이용하여 합집합과 교집합의 원소의 수를 구하는 방법이 멋졌다. 진짜 감탄밖에 나오지 않는다.. 교집합과 합집합을 만드는 좋은 방법에 대해 배웠다고 생각한다.

---

### 결과

![programmers_news_clustering_result3](/image/CodingTest/programmers_news_clustering/programmers_news_clustering_result3.png)

---

## 6. Conclusion

> 두 번째 Level2 문제였다. 시간은 많이 걸렸지만 어쨋든 내 스스로의 힘으로 풀게 되어서 뿌듯했다. 자카드 유사도라는 것도 처음으로 알게되었는데 만약 문제에 세세한 설명이 없었더라면 개념에 대해 검색하고 이해하느라 시간이 더욱 걸렸을 것이다. 문제에서 원하는 것은 자카드 유사도에 대한 개념이 아니라 자카드 유사도를 구하기 까지의 과정이 아니었나 싶다.  
> 매 문제의 `다른 사람 풀이`에서 항상 감탄을 받는데 특히 이번 문제의 `다른 사람 풀이`에서는 입이 쫙 벌어지면서 감탄을 할 수 밖에 없었다. 나도 `Set`에 대해 알고 있었지만 `Set`를 통해 합집합과 교집합을 각각 구하는 코드를 보니 정말 멋지다는 생각밖에 들지 않았다. 또한 내가 아직 효율적으로 코드 짜는 것이 부족하다고 느끼는 이유가 합집합과 교집합을 구하는 과정을 보면 내가 다 알고 있는 메서드로만 이루어져 있기 때문이다. 많은 문제를 풀어보고 많은 풀이 과정에 대해 생각하고 다른 사람의 풀이도 많이 참고해야겠다✍️

---

## 참고

[MDN - String.prototype.substr()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/substr)  
[MDN - String.prototype.substring()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/substring)  
[MDN - String.prototype.slice()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/slice)  
[MDN - String.prototype.match()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match)

---

[👆](#뉴스-클러스터링)

📅 2022-08-15
