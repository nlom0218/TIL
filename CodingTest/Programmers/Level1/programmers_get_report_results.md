# 신고 결과 받기

## 1. 개요

- 프로그래머스
- Lv.1
- 2022 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/92334)

---

## 2. 문제 설명

신입사원 무지는 게시판 불량 이용자를 신고하고 처리 결과를 메일로 발송하는 시스템을 개발하려 합니다. 무지가 개발하려는 시스템은 다음과 같습니다.

- 각 유저는 한 번에 한 명의 유저를 신고할 수 있습니다.
  - 신고 횟수에 제한은 없습니다. 서로 다른 유저를 계속해서 신고할 수 있습니다.
  - 한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리됩니다.
- k번 이상 신고된 유저는 게시판 이용이 정지되며, 해당 유저를 신고한 모든 유저에게 정지 사실을 메일로 발송합니다.
  - 유저가 신고한 모든 내용을 취합하여 마지막에 한꺼번에 게시판 이용 정지를 시키면서 정지 메일을 발송합니다.

다음은 전체 유저 목록이 ["muzi", "frodo", "apeach", "neo"]이고, k = 2(즉, 2번 이상 신고당하면 이용 정지)인 경우의 예시입니다.

| 유저 ID  | 유저가 신고한 ID | 설명                               |
| -------- | ---------------- | ---------------------------------- |
| "muzi"   | "frodo"          | "muzi"가 "frodo"를 신고했습니다.   |
| "apeach" | "frodo"          | "apeach"가 "frodo"를 신고했습니다. |
| "frodo"  | "neo"            | "frodo"가 "neo"를 신고했습니다.    |
| "muzi"   | "neo"            | "muzi"가 "neo"를 신고했습니다.     |
| "apeach" | "muzi"           | "apeach"가 "muzi"를 신고했습니다.  |

각 유저별로 신고당한 횟수는 다음과 같습니다.

| 유저 ID  | 신고당한 횟수 |
| -------- | ------------- |
| "muzi"   | 1             |
| "frodo"  | 2             |
| "apeach" | 0             |
| "neo"    | 2             |

위 예시에서는 2번 이상 신고당한 "frodo"와 "neo"의 게시판 이용이 정지됩니다. 이때, 각 유저별로 신고한 아이디와 정지된 아이디를 정리하면 다음과 같습니다.

| 유저 ID  | 유저가 신고한 ID  | 정지된 ID        |
| -------- | ----------------- | ---------------- |
| "muzi"   | ["frodo", "neo"]  | ["frodo", "neo"] |
| "frodo"  | ["neo"]           | ["neo"]          |
| "apeach" | ["muzi", "frodo"] | ["frodo"]        |
| "neo"    | 없음              | 없음             |

따라서 "muzi"는 처리 결과 메일을 2회, "frodo"와 "apeach"는 각각 처리 결과 메일을 1회 받게 됩니다.

이용자의 ID가 담긴 문자열 배열 id_list, 각 이용자가 신고한 이용자의 ID 정보가 담긴 문자열 배열 report, 정지 기준이 되는 신고 횟수 k가 매개변수로 주어질 때, 각 유저별로 처리 결과 메일을 받은 횟수를 배열에 담아 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 2 ≤ id_list의 길이 ≤ 1,000
  - 1 ≤ id_list의 원소 길이 ≤ 10
  - id_list의 원소는 이용자의 id를 나타내는 문자열이며 알파벳 소문자로만 이루어져 있습니다.
  - id_list에는 같은 아이디가 중복해서 들어있지 않습니다.
- 1 ≤ report의 길이 ≤ 200,000
  - 3 ≤ report의 원소 길이 ≤ 21
  - report의 원소는 "이용자id 신고한id"형태의 문자열입니다.
  - 예를 들어 "muzi frodo"의 경우 "muzi"가 "frodo"를 신고했다는 의미입니다.
  - id는 알파벳 소문자로만 이루어져 있습니다.
  - 이용자id와 신고한id는 공백(스페이스)하나로 구분되어 있습니다.
  - 자기 자신을 신고하는 경우는 없습니다.
- 1 ≤ k ≤ 200, k는 자연수입니다.
- return 하는 배열은 id_list에 담긴 id 순서대로 각 유저가 받은 결과 메일 수를 담으면 됩니다.

---

### 2-2. 문제 설명 - 입출력 예

| id_list                            | report                                                             | k   | result    |
| ---------------------------------- | ------------------------------------------------------------------ | --- | --------- |
| ["muzi", "frodo", "apeach", "neo"] | ["muzi frodo","apeach frodo","frodo neo","muzi neo","apeach muzi"] | 2   | [2,1,1,0] |
| ["con", "ryan"]                    | ["ryan con", "ryan con", "ryan con", "ryan con"]                   | 3   | [0,0]     |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

문제의 예시와 같습니다.

입출력 예 #2

"ryan"이 "con"을 4번 신고했으나, 주어진 조건에 따라 한 유저가 같은 유저를 여러 번 신고한 경우는 신고 횟수 1회로 처리합니다. 따라서 "con"은 1회 신고당했습니다. 3번 이상 신고당한 이용자는 없으며, "con"과 "ryan"은 결과 메일을 받지 않습니다. 따라서 [0, 0]을 return 합니다.

---

### 2-4. 문제 설명 - 제한시간 안내

- 정확성 테스트: 10초

---

## 3. 문제 풀이

```JavaScript
function solution(id_list, report, k) {
  // 1) report 배열의 요소 중 중복되는 요소 제거한 후 문자열인 각각의 요소를 배열로 만들기
  const set = [...new Set([...report])].map((item) => item.split(" "));

  // 2) 맵(Map) 객체를 사용하여 "신고당한 횟수"와 "유저가 신고한 아이디" 저장하기
  const reportedNumMap = new Map();
  const reportIdListMap = new Map();
  set.forEach((item) => {
    const [userId, reportId] = item;
    // 3) 특정 아이디(유저)가 신고당한 횟수 저장하기
    reportedNumMap.set(reportId, (reportedNumMap.get(reportId) | 0) + 1);
    // 4) 특정 아이디(유저)가 신고한 아이디 저장하기
    reportIdListMap.set(
      userId,
      reportIdListMap.get(userId)
        ? [...reportIdListMap.get(userId), reportId]
        : [reportId]
    );
  });

  // 5) 정지 기준이 되는 신고 횟수 이상인 아이디만 저장하기
  const reportedId = [];
  reportedNumMap.forEach((item, index) => {
    if (item >= k) reportedId.push(index);
  });

  // 6) id_list 배열의 순서에 맞게 각각의 요소(유저)가 신고한 ID를 넣기
  const reportIdList = id_list.map((item) => {
    return reportIdListMap.get(item) ? reportIdListMap.get(item) : 0;
  });

  // 7) 유저가 신고한 아이디들이 정지 기준에 충족하는 아이디인지 확인하기
  const resultEmailNum = reportIdList.map((item) => {
    if (item === 0) return 0;
    const suspendedIdArr = item.filter((id) => reportedId.includes(id));
    return suspendedIdArr.length;
  });

  return resultEmailNum;
}
```

---

### 1) report 배열의 요소 중 중복되는 요소 제거한 후 문자열인 각각의 요소를 배열로 만들기

```JavaScript
const set = [...new Set([...report])].map((item) => item.split(" "));
```

`report` 배열의 요소들은 "이용자id 신고한id"의 형태인 문자열이다. 같은 이용자가 동일한 유저를 여러번 신고할 수 있지만 동일한 유저에 대한 신고 횟수는 1회로 처리된다. 즉, 중복되는 요소는 제거해야 한다. 이를 위해 `셋(Set)` 객체를 사용하였고 "이용자id 신고한id"를 [이용자id, 신고한id]로 바꾸었다.

- [...new Set([...report])]: 중복되는 요소 제거한 새로운 배열
- Array.map(), String.split(): "이용자id 신고한id" -> [이용자id, 신고한id]

---

### 2) 맵(Map) 객체를 사용하여 "신고당한 횟수"와 "유저가 신고한 아이디" 저장하기

```JavaScript
const reportedNumMap = new Map();
const reportIdListMap = new Map();
```

특정 유저가 얼마만큼 신고를 당했는지에 대한 정보가 들어있는 `reportedNumMap` 맵 객체와 특정 유저가 어떤 유저들을 신고했는지에 대한 정보가 들어있는 `reportIdListMap` 맵 객체를 선언한다.

---

### 3) 특정 아이디(유저)가 신고당한 횟수 저장하기

```JavaScript
reportedNumMap.set(reportId, (reportedNumMap.get(reportId) | 0) + 1);
```

`reportedNumMap` 맵 객체에 새로운 값을 저장한다.

- key: 신고를 당한 유저의 아이디
- value: 해당 유저가 신고당한 횟수(number)

이미 어떤 아이디에 대한 신고 횟수가 있으면 그 신고 횟수에 + 1를 하면서 누적된 신고 횟수를 구한다.

---

### 4) 특정 아이디(유저)가 신고한 아이디 저장하기

```JavaScript
reportIdListMap.set(
  userId,
  reportIdListMap.get(userId)
    ? [...reportIdListMap.get(userId), reportId]
    : [reportId]
);
```

`reportIdListMap` 맵 객체에 새로운 값을 저장한다.

- key: 신고를 한 유저의 아이디
- value: 해당 유저가 신고한 아이디 목록(배열)

이미 유저를 신고한 이력이 있으면 스프레드 연산자를 이용하여 배열을 만든다.

---

### 5) 정지 기준이 되는 신고 횟수 이상인 아이디만 저장하기

```JavaScript
const reportedId = [];
reportedNumMap.forEach((item, index) => {
  if (item >= k) reportedId.push(index);
});
```

`reportedNumMap` 맵 객체에는 유저가 얼마만큼 신고가 되었는지 횟수가 저장되어 있다. 하지만 신고가 되었다 해도 정지가 모두 되는 것이 아니라 정지 기준이 되는 신고 횟수 이상의 신고를 받아야 정지가 된다.

해당 기준을 알려주는 값이 바로 `k`이다. `k`와 `reportedNumMap` 맵 객체의 각각의 값을 비교하여 정지가 되는 아이디만 가지는 `reportedId` 배열을 만든다.

---

### 6) id_list 배열의 순서에 맞게 각각의 요소(유저)가 신고한 ID를 넣기

```JavaScript
const reportIdList = id_list.map((item) => {
  return reportIdListMap.get(item) ? reportIdListMap.get(item) : 0;
});
```

문제에서 원하는 것은 `id_list`에 담겨 있는 유저들에게 몇 개의 메일을 보낼지이다. 즉, `id_list`에 담긴 유저들의 순서에 맞게 메일의 수를 배열의 요소로 넣고 배열을 리턴을 해야 하는 것이다.

이번 과정에서는 `id_list` 배열을 반복문을 돌면서 각각의 유저들이 신고한 유저들을 담은 `reportIdList` 배열을 만든다.

반복문에서 `item`은 유저이다. 그리고 `reportIdListMap` 맵 객체는 유저가 신고한 아이디(배열의 형태이다)를 값으로 가지고 있다. 이러한 내용을 바탕으로 `Map.get(key)` 메서드를 사용하여 `key`에 해당하는 유저가 신고한 아이디가 있으면 해당 아이디들을 리턴하고 신고한 아이디가 없으면 0를 리턴한다.

---

### 7) 유저가 신고한 아이디들이 정지 기준에 충족하는 아이디인지 확인하기

```JavaScript
const resultEmailNum = reportIdList.map((item) => {
  if (item === 0) return 0;
  const suspendedIdArr = item.filter((id) => reportedId.includes(id));
  return suspendedIdArr.length;
});
```

주의해야 할 점이 신고를 했다해도 무조건 정지가 되어 결과 메일이 받는 것은 아니다. 자신이 신고한 아이디가 실제 정지된 아이디인지 확인을 해야한다. 바로 이를 위한 과정을 나타낸 코드이다.

순서대로 살펴보자.

1. item: `reportIdList` 배열의 요소이며 이것 또한 문자열을 요소로 가지는 배열이다.
2. item === 0: `item` 값이 0이라면 신고한 아이디가 없으므로 0를 반환하고 다음 반복문으로 넘어간다.
3. suspendedIdArr: `item` 배열에 들어있는 유저의 아이디가 `reportedId` 배열에 포함되면 해당 배열에 저장한다. 여기에 저장된 배열이 최종적으로 정지를 당한 아이디가 된다.
4. suspendedIdArr.length: 정지를 당한 아이디 만큼 결과 메일을 받으므로 길이를 반환한다.

---

### 결과

![programmers_get_report_results_result1](/image/CodingTest/programmers_get_report_results/programmers_get_report_results_reuslt1.png)

---

## 4. Refactoring

코드의 흐름은 같다. 하지만 `7) 유저가 신고한 아이디들이 정지 기준에 충족하는 아이디인지 확인하기`에서의 아래 부분을 굳이 조건문을 쓰면서 따로 생각을 해야 하나 싶었다.

```JavaScript
if (item === 0) return 0;
```

위의 조건문을 제거하기 위해서는 `item`으로 0으로 미리 두는 것이 아니라 빈 배열로 만들어야 한다. 이는 ` 6) id_list 배열의 순서에 맞게 각각의 요소(유저)가 신고한 ID를 넣기`에서 수정해야 한다.(아래 코드 참고)

```JavaScript
return reportIdListMap.get(item) ? reportIdListMap.get(item) : [];
```

어차피 빈 배열이라는 것은 어떤 유저가 신고한 다른 유저가 없다는 것이고 마지막엔 이 배열의 길이를 반환하여 정답으로 제출한다. 즉 빈 배열의 길이는 0이므로 0이 해당 유저가 받은 결과 메일의 수가 된다.

---

## 5. 다른 사람 풀이

가장 많은 좋아요와 댓글이 달린 풀이를 가져왔다.

```JavaScript
function solution(id_list, report, k) {
  let reports = [...new Set(report)].map((a) => {
    return a.split(" ");
  });

  // 1) 누적 신고 횟수 구하기
  let counts = new Map();
  for (const report of reports) {
    counts.set(report[1], counts.get(report[1]) + 1 || 1);
  }

  // 2) 정지 기준이 되는 신고 횟수를 비교하여 받게 될 결과 메일의 수 구하기
  let good = new Map();
  for (const report of reports) {
    if (counts.get(report[1]) >= k) {
      good.set(report[0], good.get(report[0]) + 1 || 1);
    }
  }

  // 3) 받게 될 결과 메일의 수를 id_list 요소의 순서에 맞게 정렬하기
  let answer = id_list.map((a) => good.get(a) || 0);
  return answer;
}
```

---

### 1) 누적 신고 횟수 구하기

```JavaScript
let counts = new Map();
for (const report of reports) {
  counts.set(report[1], counts.get(report[1]) + 1 || 1);
}
```

나의 풀이에서 `reportedNumMap` 맵 객체를 만드는 과정과 같다.

---

### 2) 정지 기준이 되는 신고 횟수를 비교하여 받게 될 결과 메일의 수 구하기

```JavaScript
let good = new Map();
for (const report of reports) {
  if (counts.get(report[1]) >= k) {
    good.set(report[0], good.get(report[0]) + 1 || 1);
  }
}
```

나의 풀이 중 `4) 특정 아이디(유저)가 신고한 아이디 저장하기`, `5) 정지 기준이 되는 신고 횟수 이상인 아이디만 저장하기`, `7) 유저가 신고한 아이디들이 정지 기준에 충족하는 아이디인지 확인하기` 과정을 하나로 표현한 코드이다. 정확히 말하면 3개의 과정을 요약을 했다기 보다는 다른 방법으로 접근을 하였다.

1. `good` 맵 객체를 만든다.
   - key: 유저 아이디
   - value: 유저가 받게 될 결과 메일의 수
2. report[0]: 신고를 한 유져
3. report[1]: 신고를 당한 유저
4. 신고를 당한 유저(report[1])가 정지 기준이 되는 신고 횟수 이상을 받을 경우만 생각한다.
5. 신고를 한 유저(report[0])가 받게 될 결과 메일 수를 누적해서 더한 후 저장한다.

---

### 3) 받게 될 결과 메일의 수를 id_list 요소의 순서에 맞게 정렬하기

```JavaScript
let answer = id_list.map((a) => good.get(a) || 0);
```

`it_list` 배열의 요소(유저)가 받게 될 결과 메일을 구한다. 이때 `good` 맵 객체에 `key`가 없다면 0개의 결과 메일을 받게 된다. 이때 0이 되는 기준은 아래와 같다.

1. 신고한 유저가 아예 없는 경우
2. 신고한 유저가 정지 기준이 되는 신고 횟수 이상의 신고를 받지 않은 경우

---

### 결과

과정을 더 단순하게 생각해서 그런지 실행 속도가 전체적으로 빨라진 것을 확인할 수 있다.

![programmers_get_report_results_reuslt2](/image/CodingTest/programmers_get_report_results/programmers_get_report_results_reuslt2.png)

---

## 6. Conclusion

> 생각보다 긴 시간을 소요하지 않고 뚝딱 풀었다. 계속 연습하고 여러 문제를 풀다보니 얼추 느낌을 알아가는 듯 한다. 그리고 확실히 Level2 보다는 구현자체가 간단한 느낌이다. Level2 문제는 Level1 문제 2~3개가 짬뽕되어 있는 느낌이랄까? 내일 오전에 코테 스터디가 있을 텐데 아마 Level2를 풀 것이다. 시간이 오버되고 정답을 맞추지 못하더라도 끝까지 공부한다고 생각하면서 풀어보자.🙏

---

[👆](#신고-결과-받기)

📅 2022-08-15
