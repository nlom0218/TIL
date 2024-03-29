# 성격 유형 검사하기

## 1. 개요

- 프로그래머스
- Lv.1
- 2022 KAKAO TECH INTERNSHIP
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/118666)

---

## 2. 문제 설명

나만의 카카오 성격 유형 검사지를 만들려고 합니다.
성격 유형 검사는 다음과 같은 4개 지표로 성격 유형을 구분합니다. 성격은 각 지표에서 두 유형 중 하나로 결정됩니다.

| 지표 번호 | 성격 유형              |
| --------- | ---------------------- |
| 1번 지표  | 라이언형(R), 튜브형(T) |
| 2번 지표  | 콘형(C), 프로도형(F)   |
| 3번 지표  | 제이지형(J), 무지형(M) |
| 4번 지표  | 어피치형(A), 네오형(N) |

4개의 지표가 있으므로 성격 유형은 총 16(=2 x 2 x 2 x 2)가지가 나올 수 있습니다. 예를 들어, "RFMN"이나 "TCMA"와 같은 성격 유형이 있습니다.

검사지에는 총 n개의 질문이 있고, 각 질문에는 아래와 같은 7개의 선택지가 있습니다.

- 매우 비동의
- 비동의
- 약간 비동의
- 모르겠음
- 약간 동의
- 동의
- 매우 동의

각 질문은 1가지 지표로 성격 유형 점수를 판단합니다.

예를 들어, 어떤 한 질문에서 4번 지표로 아래 표처럼 점수를 매길 수 있습니다.

| 선택지      | 성격 유형 점수                        |
| ----------- | ------------------------------------- |
| 매우 비동의 | 네오형 3점                            |
| 비동의      | 네오형 2점                            |
| 약간 비동의 | 네오형 1점                            |
| 모르겠음    | 어떤 성격 유형도 점수를 얻지 않습니다 |
| 약간 동의   | 어피치형 1점                          |
| 동의        | 어피치형 2점                          |
| 매우 동의   | 어피치형 3점                          |

이때 검사자가 질문에서 약간 동의 선택지를 선택할 경우 어피치형(A) 성격 유형 1점을 받게 됩니다. 만약 검사자가 매우 비동의 선택지를 선택할 경우 네오형(N) 성격 유형 3점을 받게 됩니다.

위 예시처럼 네오형이 비동의, 어피치형이 동의인 경우만 주어지지 않고, 질문에 따라 네오형이 동의, 어피치형이 비동의인 경우도 주어질 수 있습니다.
하지만 각 선택지는 고정적인 크기의 점수를 가지고 있습니다.

- 매우 동의나 매우 비동의 선택지를 선택하면 3점을 얻습니다.
- 동의나 비동의 선택지를 선택하면 2점을 얻습니다.
- 약간 동의나 약간 비동의 선택지를 선택하면 1점을 얻습니다.
- 모르겠음 선택지를 선택하면 점수를 얻지 않습니다.

검사 결과는 모든 질문의 성격 유형 점수를 더하여 각 지표에서 더 높은 점수를 받은 성격 유형이 검사자의 성격 유형이라고 판단합니다. 단, 하나의 지표에서 각 성격 유형 점수가 같으면, 두 성격 유형 중 사전 순으로 빠른 성격 유형을 검사자의 성격 유형이라고 판단합니다.

질문마다 판단하는 지표를 담은 1차원 문자열 배열 survey와 검사자가 각 질문마다 선택한 선택지를 담은 1차원 정수 배열 choices가 매개변수로 주어집니다. 이때, 검사자의 성격 유형 검사 결과를 지표 번호 순서대로 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 1 ≤ survey의 길이 ( = n) ≤ 1,000
  - survey의 원소는 "RT", "TR", "FC", "CF", "MJ", "JM", "AN", "NA" 중 하나입니다.
  - survey[i]의 첫 번째 캐릭터는 i+1번 질문의 비동의 관련 선택지를 선택하면 받는 성격 유형을 의미합니다.
  - survey[i]의 두 번째 캐릭터는 i+1번 질문의 동의 관련 선택지를 선택하면 받는 성격 유형을 의미합니다.
- choices의 길이 = survey의 길이
  - choices[i]는 검사자가 선택한 i+1번째 질문의 선택지를 의미합니다.
  - 1 ≤ choices의 원소 ≤ 7

| choices | 뜻          |
| ------- | ----------- |
| 1       | 매우 비동의 |
| 2       | 비동의      |
| 3       | 약간 비동의 |
| 4       | 모르겠음    |
| 5       | 약간 동의   |
| 6       | 동의        |
| 7       | 매우 동의   |

---

### 2-2. 문제 설명 - 입출력 예

| survey                         | choices         | result |
| ------------------------------ | --------------- | ------ |
| ["AN", "CF", "MJ", "RT", "NA"] | [5, 3, 2, 7, 5] | "TCMA" |
| ["TR", "RT", "TR"]             | [7, 1, 3]       | "RCJA" |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

1번 질문의 점수 배치는 아래 표와 같습니다.

| 선택지      | 성격 유형 점수                        |
| ----------- | ------------------------------------- |
| 매우 비동의 | 어피치형 3점                          |
| 비동의      | 어피치형 2점                          |
| 약간 비동의 | 어피치형 1점                          |
| 모르겠음    | 어떤 성격 유형도 점수를 얻지 않습니다 |
| 약간 동의   | 네오형 1점                            |
| 동의        | 네오형 2점                            |
| 매우 동의   | 네오형 3점                            |

1번 질문에서는 지문의 예시와 다르게 비동의 관련 선택지를 선택하면 어피치형(A) 성격 유형의 점수를 얻고, 동의 관련 선택지를 선택하면 네오형(N) 성격 유형의 점수를 얻습니다.
1번 질문에서 검사자는 약간 동의 선택지를 선택했으므로 네오형(N) 성격 유형 점수 1점을 얻게 됩니다.

2번 질문의 점수 배치는 아래 표와 같습니다.

| 선택지               | 성격 유형 점수                        |
| -------------------- | ------------------------------------- |
| 매우 비동의          | 콘형 3점                              |
| 비동의               | 콘형 2점                              |
| 약간 비동의 콘형 1점 |
| 모르겠음             | 어떤 성격 유형도 점수를 얻지 않습니다 |
| 약간 동의            | 프로도형 1점                          |
| 동의                 | 프로도형 2점                          |
| 매우 동의            | 프로도형 3점                          |

2번 질문에서 검사자는 약간 비동의 선택지를 선택했으므로 콘형(C) 성격 유형 점수 1점을 얻게 됩니다.

3번 질문의 점수 배치는 아래 표와 같습니다.

| 선택지      | 성격 유형 점수                        |
| ----------- | ------------------------------------- |
| 매우 비동의 | 무지형 3점                            |
| 비동의      | 무지형 2점                            |
| 약간 비동의 | 무지형 1점                            |
| 모르겠음    | 어떤 성격 유형도 점수를 얻지 않습니다 |
| 약간 동의   | 제이지형 1점                          |
| 동의        | 제이지형 2점                          |
| 매우 동의   | 제이지형 3점                          |

3번 질문에서 검사자는 비동의 선택지를 선택했으므로 무지형(M) 성격 유형 점수 2점을 얻게 됩니다.

4번 질문의 점수 배치는 아래 표와 같습니다.

| 선택지      | 성격 유형 점수                        |
| ----------- | ------------------------------------- |
| 매우 비동의 | 라이언형 3점                          |
| 비동의      | 라이언형 2점                          |
| 약간 비동의 | 라이언형 1점                          |
| 모르겠음    | 어떤 성격 유형도 점수를 얻지 않습니다 |
| 약간 동의   | 튜브형 1점                            |
| 동의        | 튜브형 2점                            |
| 매우 동의   | 튜브형 3점                            |

4번 질문에서 검사자는 매우 동의 선택지를 선택했으므로 튜브형(T) 성격 유형 점수 3점을 얻게 됩니다.

5번 질문의 점수 배치는 아래 표와 같습니다.

| 선택지      | 성격 유형 점수                        |
| ----------- | ------------------------------------- |
| 매우 비동의 | 네오형 3점                            |
| 비동의      | 네오형 2점                            |
| 약간 비동의 | 네오형 1점                            |
| 모르겠음    | 어떤 성격 유형도 점수를 얻지 않습니다 |
| 약간 동의   | 어피치형 1점                          |
| 동의        | 어피치형 2점                          |
| 매우 동의   | 어피치형 3점                          |

5번 질문에서 검사자는 약간 동의 선택지를 선택했으므로 어피치형(A) 성격 유형 점수 1점을 얻게 됩니다.

1번부터 5번까지 질문의 성격 유형 점수를 합치면 아래 표와 같습니다.

| 지표 번호 | 성격 유형   | 점수 | 성격 유형   | 점수 |
| --------- | ----------- | ---- | ----------- | ---- |
| 1번 지표  | 라이언형(R) | 0    | 튜브형(T)   | 3    |
| 2번 지표  | 콘형(C)     | 1    | 프로도형(F) | 0    |
| 3번 지표  | 제이지형(J) | 0    | 무지형(M)   | 2    |
| 4번 지표  | 어피치형(A) | 1    | 네오형(N)   | 1    |

각 지표에서 더 점수가 높은 T,C,M이 성격 유형입니다.
하지만, 4번 지표는 1점으로 동일한 점수입니다. 따라서, 4번 지표의 성격 유형은 사전순으로 빠른 A입니다.

따라서 "TCMA"를 return 해야 합니다.

입출력 예 #2

1번부터 3번까지 질문의 성격 유형 점수를 합치면 아래 표와 같습니다.

| 지표 번호 | 성격 유형   | 점수 | 성격 유형   | 점수 |
| --------- | ----------- | ---- | ----------- | ---- |
| 1번 지표  | 라이언형(R) | 6    | 튜브형(T)   | 1    |
| 2번 지표  | 콘형(C)     | 0    | 프로도형(F) | 0    |
| 3번 지표  | 제이지형(J) | 0    | 무지형(M)   | 0    |
| 4번 지표  | 어피치형(A) | 0    | 네오형(N)   | 0    |

1번 지표는 튜브형(T)보다 라이언형(R)의 점수가 더 높습니다. 따라서 첫 번째 지표의 성격 유형은 R입니다.
하지만, 2, 3, 4번 지표는 모두 0점으로 동일한 점수입니다. 따라서 2, 3, 4번 지표의 성격 유형은 사전순으로 빠른 C, J, A입니다.

따라서 "RCJA"를 return 해야 합니다.

---

## 3. 문제 풀이

```javascript
function solution(survey, choices) {
  // 1) 맵(Map) 객체 생성
  const map = new Map();

  survey.forEach((item, index) => {
    // 2) item의 첫 번째 글자, 두 번째 글자 가져오기
    const [a, b] = item;

    // 3) 기준이 되는 점수 구하기
    const score = 4 - choices[index];

    if (score === 0) {
      // 4) score가 0점이면 다음 반복문으로 이동
      return;
    } else if (score > 0) {
      // 5) score가 양수이면 해당 score를 a에게 추가
      map.set(a, (map.get(a) | 0) + score);
    } else {
      // 6) score가 음수이면 해당 score(절댓값)를 b에게 추가
      map.set(b, (map.get(b) | 0) + Math.abs(score));
    }
  });

  // 7) 같은 지표에 있는 성격 유형을 비교하여 최종 성격 유형 구하기
  return [
    (map.get("R") | 0) >= (map.get("T") | 0) ? "R" : "T",
    (map.get("C") | 0) >= (map.get("F") | 0) ? "C" : "F",
    (map.get("J") | 0) >= (map.get("M") | 0) ? "J" : "M",
    (map.get("A") | 0) >= (map.get("N") | 0) ? "A" : "N",
  ].join("");
}
```

---

### 1) 맵(Map) 객체 생성

```javascript
const map = new Map();
```

8가지의 성격유형은 선택지의 결과에 따라 각각 점수를 얻는다. 점수가 누적되어 마지막에는 높은 점수가 최종 성격유형으로 선택된다.
점수의 누적과 고유한 성격유형을 관리하기 위해 각각의 성격유형을 키로 설정하고 점수를 값으로 설정한다. 배열과 객체보다는 맵(Map) 객체를
이용하여 중복된 키를 가지지 않도록 한다.

---

### 2) item의 첫 번째 글자, 두 번째 글자 가져오기

```javascript
const [a, b] = item;
```

`survey` 배열의 요소는 질문마다 판단하는 지표를 담은 문자열이다. 첫 번째 문자는 동의이고 두 번째 문자는 비동의이다. `choices`의
배열에 있는 요소에 따라서 선택되는 지표와 점수가 달라지므로 `survey` 배열의 요소인 문자열을 두 개로 나누어 관리한다.

문자열도 iterable하기 때문에 구조분해추가을 통해 문자를 하나씩 가져올 수 있다.

---

### 3) 기준이 되는 점수 구하기

```javascript
const score = 4 - choices[index];
```

기준이 되는 점수를 구하는 과정이다. 만약 `choices` 배열의 요소는 1~7 사이의 숫자이며 중간 값이 4이다.
4에서 `choices` 배열의 요소를 빼 나온 값을 바탕으로 동의했는지 비동의했는지를 파악할 수 있으며 부여하는 점수도 알 수 있다.

---

### 4) score가 0점이면 다음 반복문으로 이동

```javascript
if (score === 0) {
  return;
}
```

`score`가 0이라는 것은 4를 선택했다는 것이고 이때는 어떤한 성격 유형에 점수를 추가할 수 없으므로 다음 반복문으로 넘어가야 한다.

---

### 5) score가 양수이면 해당 score를 a에게 추가

```javascript
else if (score > 0) {
    map.set(a, (map.get(a) | 0) + score);
}
```

`score`가 양수라는 것은 1~3를 선택했다는 것이고 이는 비동의를 한 것이다. 그러므로 `score` 만큼의 점수를 `a`에 해당하는 성격유형에
추가한다.

---

### 6) score가 음수이면 해당 score(절댓값)를 b에게 추가

```javascript
else {
    map.set(b, (map.get(b) | 0) + Math.abs(score));
}
```

마지막으로 `socre`가 음수라는 것은 5~7를 선택한 것이고 이는 동의를 한 것이다. 그러므로 `socre`의 절댓값 만큼의 점수를 `b`에
해당하는 성격유형에 추가한다.

---

### 7) 같은 지표에 있는 성격 유형을 비교하여 최종 성격 유형 구하기

```javascript
return [
  (map.get("R") | 0) >= (map.get("T") | 0) ? "R" : "T",
  (map.get("C") | 0) >= (map.get("F") | 0) ? "C" : "F",
  (map.get("J") | 0) >= (map.get("M") | 0) ? "J" : "M",
  (map.get("A") | 0) >= (map.get("N") | 0) ? "A" : "N",
].join("");
```

같은 지표에 있는 성격 유형의 점수를 비교하여 높은 점수를 얻은 성격 유형을 최종적으로 선택한다. 이때 만약 점수가 같다면 사전순 빠른 이름의
성격유형을 선택한다. 또한 점수를 아예 못 받은 성격유형도 있을 수 있으므로 `map.get()`으로 가져온 값이 없으면 0으로 정하여 비교한다.

마지막으로 배열을 문자열로 바꾸기 위해 `Array.join()` 메서드를 사용한다.

---

### 결과

![programmers_test_personality_type_result](/image/CodingTest/programmers_test_personality_type/programmers_test_personality_type_result1.png)

---

## 4. 다른 사람 풀이

2022년 8월 20일 기준 해당 문제가 프로그래머스에 올라온지 오래되지 않았기 때문에 좋아요를 받은, 댓글이 많은 문제는 없다.
다만 여러 문제를 살펴봤을 때 대부분 맵(Map) 객체를 사용하여 해결하였다.

몇몇개의 풀이에서는 객체를 사용하여 푼 풀이도 보인다. 그 중 하나를 가져왔다. `김시은`님의 풀이이다.

```javascript
function solution(survey, choices) {
  // 검사 개수
  const n = survey.length;
  // 검사 결과
  const result = {
    R: 0,
    T: 0,
    C: 0,
    F: 0,
    J: 0,
    M: 0,
    A: 0,
    N: 0,
  };

  for (let i = 0; i < n; i++) {
    // 검사 지표
    const [disagree, agree] = survey[i].split("");
    // 무효
    if (choices[i] === 4) continue;
    // 비동의
    if (choices[i] < 4) result[disagree] += 4 - (choices[i] % 4);
    // 동의
    else result[agree] += choices[i] % 4;
  }

  // 검사 결과
  let answer = "";

  answer += result["R"] >= result["T"] ? "R" : "T";
  answer += result["C"] >= result["F"] ? "C" : "F";
  answer += result["J"] >= result["M"] ? "J" : "M";
  answer += result["A"] >= result["N"] ? "A" : "N";

  return answer;
}
```

`result`객체를 만들고 `survey` 배열의 요소에 따라 동의하는지 비동의하는지 파악한다. 그리고 `result[disagree]` 이런식으로
객체의 값을 변경하고 있다. 맵을 사용했는지 객체를 사용했는지만 다르고 전체적인 로직은 같다.

멋진 점은 검사 지표를 구조분해할당을 할 때 난 a, b로 변수명을 정했지만 위의 풀이에서는 disagree, agree로 정했다는 것이다.
보다 의미를 알 수 있으니 가독성도 좋아보인다. 또한 이후 조건문을 작성할 때 헷갈리지 않을 것 같다.(`.split("")`은 생략해도 된다.)

---

## 5. Conclusion

> 처음 문제를 봤을 때 문제 설명의 길이가 길어 겁이 났지만 생각보다 쉽게 문제를 풀었다. 10~15분 정도 걸린 듯 하다. 아마
> 코딩 테스트 공부를 시작한 이후로 꾸준히 문제를 풀고 문제 분석을 한 보람이지 않을까 싶다? 물론 2단계로 넘어가기만 하면 3시간은 기본이다.ㅎㅎ
> 계속 꾸준히 공부하자. 개학 이후에도 일주일에 적어도 3문제는 풀고 분석하는 시간을 가지도록 하자.

---

📅 2022-08-20
