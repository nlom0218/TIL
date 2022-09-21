# 주차 요금 계산

## 1. 개요

- 프로그래머스
- Lv.2
- 2022 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/92341)

---

## 2. 문제 설명

주차장의 요금표와 차량이 들어오고(입차) 나간(출차) 기록이 주어졌을 때, 차량별로 주차 요금을 계산하려고 합니다. 아래는 하나의 예시를 나타냅니다.

- 요금표
  |기본 시간(분)| 기본 요금(원)| 단위 시간(분)| 단위 요금(원)|
  |---|---|---|---|
  |180 |5000| 10| 600|

- 입/출차 기록
  |시각(시:분) |차량 번호 |내역|
  |---|---|---|
  |05:34| 5961 |입차|
  |06:00| 0000| 입차|
  |06:34| 0000| 출차|
  |07:59| 5961| 출차|
  |07:59| 0148| 입차|
  |18:59 |0000| 입차|
  |19:09 |0148| 출차|
  |22:59 |5961| 입차|
  |23:00 |5961| 출차|

- 자동차별 주차 요금
  |차량 번호| 누적 주차 시간(분)| 주차 요금(원)|
  |---|---|---|
  |0000 |34 + 300 = 334| 5000 + ⌈(334 - 180) / 10⌉ x 600 = 14600|
  |0148 |670| 5000 +⌈(670 - 180) / 10⌉x 600 = 34400|
  |5961 |145 + 1 = 146| 5000|

- 어떤 차량이 입차된 후에 출차된 내역이 없다면, 23:59에 출차된 것으로 간주합니다. - 0000번 차량은 18:59에 입차된 이후, 출차된 내역이 없습니다. 따라서, 23:59에 출차된 것으로 간주합니다.
  - 00:00부터 23:59까지의 입/출차 내역을 바탕으로 차량별 누적 주차 시간을 계산하여 요금을 일괄로 정산합니다.
- 누적 주차 시간이 기본 시간이하라면, 기본 요금을 청구합니다.
- 누적 주차 시간이 기본 시간을 초과하면, 기본 요금에 더해서, 초과한 시간에 대해서 단위 시간 마다 단위 요금을 청구합니다.
  - 초과한 시간이 단위 시간으로 나누어 떨어지지 않으면, 올림합니다.
  - ⌈a⌉ : a보다 작지 않은 최소의 정수를 의미합니다. 즉, 올림을 의미합니다.

주차 요금을 나타내는 정수 배열 fees, 자동차의 입/출차 내역을 나타내는 문자열 배열 records가 매개변수로 주어집니다. 차량 번호가 작은 자동차부터 청구할 주차 요금을 차례대로 정수 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- fees의 길이 = 4
  - fees[0] = 기본 시간(분)
  - 1 ≤ fees[0] ≤ 1,439
  - fees[1] = 기본 요금(원)
  - 0 ≤ fees[1] ≤ 100,000
  - fees[2] = 단위 시간(분)
  - 1 ≤ fees[2] ≤ 1,439
  - fees[3] = 단위 요금(원)
  - 1 ≤ fees[3] ≤ 10,000
- 1 ≤ records의 길이 ≤ 1,000
  - records의 각 원소는 "시각 차량번호 내역" 형식의 문자열입니다.
  - 시각, 차량번호, 내역은 하나의 공백으로 구분되어 있습니다.
  - 시각은 차량이 입차되거나 출차된 시각을 나타내며, HH:MM 형식의 길이 5인 문자열입니다.
    - HH:MM은 00:00부터 23:59까지 주어집니다.
    - 잘못된 시각("25:22", "09:65" 등)은 입력으로 주어지지 않습니다.
  - 차량번호는 자동차를 구분하기 위한, `0'~'9'로 구성된 길이 4인 문자열입니다.
  - 내역은 길이 2 또는 3인 문자열로, IN 또는 OUT입니다. IN은 입차를, OUT은 출차를 의미합니다.
  - records의 원소들은 시각을 기준으로 오름차순으로 정렬되어 주어집니다.
  - records는 하루 동안의 입/출차된 기록만 담고 있으며, 입차된 차량이 다음날 출차되는 경우는 입력으로 주어지지 않습니다.
  - 같은 시각에, 같은 차량번호의 내역이 2번 이상 나타내지 않습니다.
  - 마지막 시각(23:59)에 입차되는 경우는 입력으로 주어지지 않습니다.
  - 아래의 예를 포함하여, 잘못된 입력은 주어지지 않습니다.
    - 주차장에 없는 차량이 출차되는 경우
    - 주차장에 이미 있는 차량(차량번호가 같은 차량)이 다시 입차되는 경우

---

### 2-2. 문제 설명 - 입출력 예

| fees                 | records                                                                                                                                                       | result               |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| [180, 5000, 10, 600] | ["05:34 5961 IN", "06:00 0000 IN", "06:34 0000 OUT", "07:59 5961 OUT", "07:59 0148 IN", "18:59 0000 IN", "19:09 0148 OUT", "22:59 5961 IN", "23:00 5961 OUT"] | [14600, 34400, 5000] |
| [120, 0, 60, 591]    | ["16:00 3961 IN","16:00 0202 IN","18:00 3961 OUT","18:00 0202 OUT","23:58 3961 IN"]                                                                           | [0, 591]             |
| [1, 461, 1, 10]      | ["00:00 1234 IN"]                                                                                                                                             | [14841]              |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

- 요금표
  |기본 시간(분) |기본 요금(원)| 단위 시간(분)| 단위 요금(원)|
  |---|---|---|---|
  |120 |0 |60 |591|

- 입/출차 기록
  |시각(시:분)| 차량 번호| 내역|
  |---|---|---|
  |16:00 |3961| 입차|
  |16:00 |0202| 입차|
  |18:00 |3961| 출차|
  |18:00 |0202| 출차|
  |23:58 |3961| 입차|

- 자동차별 주차 요금
  |차량 번호 |누적 주차 시간(분)| 주차 요금(원)|
  |---|---|---|
  |0202| 120| 0|
  |3961| 120 + 1 = 121| 0 +⌈(121 - 120) / 60⌉x 591 = 591|

- 3961번 차량은 2번째 입차된 후에는 출차된 내역이 없으므로, 23:59에 출차되었다고 간주합니다.

입출력 예 #3

- 요금표
  |기본 시간(분) |기본 요금(원)| 단위 시간(분)| 단위 요금(원)|
  |---|---|---|---|
  |1 |461| 1| 10|

- 입/출차 기록
  |시각(시:분)| 차량 번호| 내역|
  |---|---|---|
  |00:00 |1234 |입차|

- 자동차별 주차 요금
  |차량 번호| 누적 주차 시간(분) |주차 요금(원)|
  |---|---|---|
  |1234| 1439| 461 +⌈(1439 - 1) / 1⌉x 10 = 14841|

- 1234번 차량은 출차 내역이 없으므로, 23:59에 출차되었다고 간주합니다.

---

### 2-4. 문제 설명 - 제한시간 안내

정확성 테스트 : 10초

---

## 3. 문제 풀이

```javascript
function solution(fees, records) {
  // 1) fee 배열의 요소를 변수에 저장하기
  const [default_time, default_pay, unit_time, unit_pay] = fees;
  const map = new Map();
  // 2) recodrs 배열을 순회 돌면서 map 객체에 차량별 입차, 출차 시간 저장하기
  records.forEach((item) => {
    // 3) recodrs 배열의 요소를 공백을 기준으로 배열 만들기
    let [time, num, type] = item.split(" ");
    // 4) 제공된 시간 정보를 다른 형태의 데이터로 바꾸기
    const [hour, minute] = time.split(":");
    time = Number(hour) * 60 + Number(minute);
    // 5) 새로운 시간 정보가 담긴 값으로 바꾸기
    const recode = map.get(num) || [];
    map.set(num, [...recode, time]);
  });
  // 6) result 배열을 만들고 차량 번호을 기준으로 오름차순 정렬하기
  const result = [];
  for ([key, value] of map) {
    result.push([key, value]);
  }
  result.sort((a, b) => a[0] - b[0]);
  return result.map(([_, recode]) => {
    // 7) 기록된 시간이 홀수개라면 24시59분에 해당하는 시간을 넣기
    if (recode.length % 2 === 1) recode.push(23 * 60 + 59);
    // 8) 반복문을 돌며 주차장을 사용한 시간이 담긴 use_time 배열 만들기
    const use_time = [];
    while (recode.length !== 0) {
      const a = recode.pop();
      const b = recode.pop();
      const time = a - b;
      use_time.push(time);
    }
    // 9) 총 이용 시간 구하기
    const cum_time = use_time.reduce((a, b) => a + b) - default_time;
    // 10) 총 이용 시간이 0과 같거나 작은 경우 기본 요금만 지불하기
    if (cum_time <= 0) {
      return default_pay;
    } else {
      // 11) 초과된 시간에 따른 주차요금에 기본 요금 더한 값을 지불하기
      return Math.ceil(cum_time / unit_time) * unit_pay + default_pay;
    }
  });
}
```

### 1) fee 배열의 요소를 변수에 저장하기

```javascript
const [default_time, default_pay, unit_time, unit_pay] = fees;
```

`fees` 배열에는 순서대로 기본시간, 기본요금, 단위시간, 단위요금을 나타내는 요소가 들어있다. 이를 구조분해 할당을 하여
각각 변수에 저장한다. 이 변수는 당장은 사용하지 않고 문제 풀이 기준 `return`에서 사용된다.

### 2) recodrs 배열을 순회 돌면서 map 객체에 차량별 입차, 출차 시간 저장하기

```javascript
records.forEach((item) => {});
```

`recodrs`에는 차량별 입차, 출차 시간이 요소로 저장되어 있다. 해당 요소를 순회를 하면서 `map` 맵 객체에 저장을 한다. 이때 맵 객체의 `key`는
차량번호가 되고 `value`는 해당 차량이 입차 또는 출차 한 시간이 된다.

---

### 3) recodrs 배열의 요소를 공백을 기준으로 배열 만들기

```javascript
let [time, num, type] = item.split(" ");
```

`recodrs` 배열의 요소는 문자열이다. 문자열엔 공백을 기준으로 `시각`, `차량번호`, `IN 또는 OUT`으로 나눌 수 있다. 이를 이용하여
`Array.split()` 메서드를 통해 요소가 3개인 배열을 만들고 구조분해 할당으로 각각의 값을 변수에 저장을 한다.

---

### 4) 제공된 시간 정보를 다른 형태의 데이터로 바꾸기

```javascript
const [hour, minute] = time.split(":");
time = Number(hour) * 60 + Number(minute);
```

문제에서 시각은 `HH:MM`의 형태로 주어진다. 그렇기 때문에 가운데의 `:`를 기준으로 나누어 배열을 만든다.
그 중 0번째 인덱스는 `hour`이고 1번째 인덱스는 `minute`를 나타낸다. 시각을 분으로 나타내기 위해
`hour`에 60을 곱한 수에 `minute`를 더한다.

---

### 5) 새로운 시간 정보가 담긴 값으로 바꾸기

```javascript
const recode = map.get(num) || [];
map.set(num, [...recode, time]);
```

먼저 맵 객체에 차량번호에 해당하는 값이 있는지 찾아본다. 만약 있으면 그 값을 `recode` 변수에 할당하고
없으면 빈 객체를 `reocde` 변수에 할당한다.

`recdoe` 변수는 배열이며 요소는 키에 해당하는 차량이 입차, 출차 한 시간이다. 항상 입차가 먼저이고 그 다음은 출차이다.
즉, 인덱스 번호를 기준으로 짝수 인덱스는 자동차가 주차장에 들어온 시간이고 홀수 인덱스는 자동차가 주차장에서 빠져나간 시간이다.

---

### 6) result 배열을 만들고 차량 번호을 기준으로 오름차순 정렬하기

```javascript
const result = [];
for ([key, value] of map) {
  result.push([key, value]);
}
result.sort((a, b) => a[0] - b[0]);
```

`map` 맵 객체를 `for..of 문`을 통해 순회한다. `key`는 차량 번호가 되며 `value`는 그 차량이 주창에 입차, 출차한 시간을
요소로 가지는 배열이다. 이런 `key`와 `value`를 요소로 가지는 배열을 `result` 배열에 넣는다.

그 후 `Array.sort()` 메서드를 사용하여 각 요소의 첫 번째 인덱스인 차량 번호을 기준으로 오름차순 정렬한다.

---

### 7) 기록된 시간이 홀수개라면 24시59분에 해당하는 시간을 넣기

```javascript
if (recode.length % 2 === 1) recode.push(23 * 60 + 59);
```

어떤 차량이 주차장을 이용할 때 입차한 시간, 출차한 시간을 저장하고 있는 `recode` 배열의 길이가 홀수라는 것은
들어간 시간을 있는데 아직 나오지 않았다는 뜻이다. 즉, 이럴 당일 끝까지 주차장을 사용하고 있다는 것이다.

문제의 조건에 따라 위와 같은 경우은 출차된 내역이 없으므로 23시59분에 출차된 것으로 간주한다. 그러므로 `recode` 배열에
23시59분에 해당하는 분을 계산하여 마지막 요소로 추가한다.

---

### 8) 반복문을 돌며 주차장을 사용한 시간이 담긴 use_time 배열 만들기

```javascript
const use_time = [];
while (recode.length !== 0) {
  const a = recode.pop();
  const b = recode.pop();
  const time = a - b;
  use_time.push(time);
}
```

`recode` 배열의 요소를 뒤에서 부터 꺼낸다. 두 개를 꺼내어 먼저 꺼낸 값(a)에서 나중에 꺼낸 값(b)을 뺀다.
`recode` 배열이 만들어진 과정을 보면 인덱스의 번호가 커질 수록 요소의 값도 커진다. 즉, 점점 시간이 커지고 있다는 것이다.

다시 `a - b`의 값은 자동차가 주차장을 이용한 시간이 된다. 이 시간을 `use_time`에 저장을 한다. 이런 반복을 `recode` 배열의 길이가
0이 될 때까지 한다.

---

### 9) 총 이용 시간 구하기

```javascript
const cum_time = use_time.reduce((a, b) => a + b) - default_time;
```

총 이용 시간을 구한다. `use_time` 배열의 모든 요소의 합을 구 하고 기본으로 주어지는 시간인 `default_time`를 뺀다.
엄밀히 말하면 `cum_time`은 기본 이용 시간에서 초과된 사용 시간을 나타낸다. 미리 `defalut_time`를 뺀 이유는 조건의 길이를
줄이기 위함이다. 리팩토링에서는 다르게 코드를 작성해보겠다.

---

### 10) 총 이용 시간이 0과 같거나 작은 경우 기본 요금만 지불하기

```javascript
if (cum_time <= 0) {
  return default_pay;
}
```

위와 같은 경우는 기본 이용 시간 이하로 주차장을 사용했다는 것이다. 그러므로 기본 이용 금액만 지불된다.

---

### 11) 초과된 시간에 따른 주차요금에 기본 요금 더한 값을 지불하기

```javascript
return Math.ceil(cum_time / unit_time) * unit_pay + default_pay;
```

`cum_time`은 기본 이용 시간에서 초과된 시간이다. 이 시간을 단위 시간을 나눈 값에 단위 요금을 더한 값이 바로
추가 요금이다. 또한 여기에 기본 요금까지 더하면 해당 차량이 지불해야 할 주차 비용이 된다.

---

### 결과

![programmers_parking_fee_calculation_result1](/image/CodingTest/programmers_parking_fee_calculation/programmers_parking_fee_calculation_result1.png)

---

## 4. Refactoring

나의 문제 풀이를 보면 `IN`, `OUT`은 사용이 되지 않았다. 이를 활용하지 않았다는 뜻이다. 이를 활용하여 문제를
다시 풀었다. 주차장에 자동차가 들어오면 `IN`이고 이때의 시간은 사용시간에서 빼기를 하고 주차장에 자동차가
들어오면 `OUT`이고 이때의 시간은 사용시간에 더하기를 한다.

그렇기 총 사용시간을 구한다. 만약 총 사용시간이 음수라면 23:59까지 출차를 하지 않는 것이므로 23:59에 해당하는
값을 최종적으로 더해준다.

또한 `result` 배열을 만드는 과정에서 자동차가 지불해야 할 주차요금을 계산하여 요소로 넣었다.

```javascript
function solution(fees, records) {
  const [default_time, default_pay, unit_time, unit_pay] = fees;
  const map = new Map();
  records.forEach((item) => {
    let [time, num, type] = item.split(" ");
    type = type === "IN" ? -1 : 1;
    const [hour, minute] = time.split(":");
    time = (Number(hour) * 60 + Number(minute)) * type;
    const recode = (map.get(num) || 0) + time;
    map.set(num, recode);
  });
  const result = [];
  for ([key, value] of map) {
    let price;
    if (value <= 0) value += 23 * 60 + 59;
    if (value <= default_time) price = default_pay;
    else {
      price =
        Math.ceil((value - default_time) / unit_time) * unit_pay + default_pay;
    }
    result.push([key, price]);
  }
  result.sort((a, b) => a[0] - b[0]);
  return result.map(([_, price]) => price);
}
```

---

### 결과

![programmers_parking_fee_calculation_result2](/image/CodingTest/programmers_parking_fee_calculation/programmers_parking_fee_calculation_result2.png)

실행결과는 크게 달라지지 않았지만 문제에서 제공하는 조건을 잘 활용하였기에 더 좋은 풀이라고 생각한다.

---

## 5. 다른 사람 풀이

좋아요를 가장 많이 받은 풀이를 가져왔다.

```javascript
function solution(fees, records) {
  const parkingTime = {};
  records.forEach((r) => {
    let [time, id, type] = r.split(" ");
    let [h, m] = time.split(":");
    time = h * 1 * 60 + m * 1;
    // 1) 차량번호가 없으면 기본 값으로 0을 할당하기
    if (!parkingTime[id]) parkingTime[id] = 0;
    // 2) IN, OUT에 따라 기존 값에 시간을 더하거나 빼기
    if (type === "IN") parkingTime[id] += 1439 - time;
    if (type === "OUT") parkingTime[id] -= 1439 - time;
  });
  const answer = [];
  // 3) Object.entries 사용하기
  for (let [car, time] of Object.entries(parkingTime)) {
    if (time <= fees[0]) price = fees[1];
    else price = Math.ceil((time - fees[0]) / fees[2]) * fees[3] + fees[1];
    answer.push([car, price]);
  }
  return answer.sort((a, b) => a[0] - b[0]).map((v) => v[1]);
}
```

전체적인 흐름은 같다. 하지만 맵 객체를 사용하지 않고 일반 객체를 사용하였고 `IN`, `OUT`에 따른
시간 계산을 다른 방법으로 하였다. 이에 대해 정리하고자 한다.

---

### 1) 차량번호가 없으면 기본 값으로 0을 할당하기

```javascript
if (!parkingTime[id]) parkingTime[id] = 0;
```

`parkingTime` 객체에 `id`에 키에 대한 값이 없으면 기본 시간으로 0을 할당한다.

---

### 2) IN, OUT에 따라 기존 값에 시간을 더하거나 빼기

```javascript
if (type === "IN") parkingTime[id] += 1439 - time;
if (type === "OUT") parkingTime[id] -= 1439 - time;
```

`1439`의 의미부터 생각하자. 이는 `23 * 60 + 59`다. 이것은 23시59분으로 자동차 주차 비용이 계산되는
마지막 시간이다. 이 값에 출차 또는 입차된 시간을 뺀 값을 `IN`일 땐 기존 시간에 더하고 `OUT`일 땐 기존 시간에 뺀다.

이는 수직선으로 생각하면 더 이해하기 쉽다.

![programmers_parking_fee_calculation](/image/CodingTest/programmers_parking_fee_calculation/programmers_parking_fee_calculation_arg1.jpeg)

---

### 3) Object.entries 사용하기

```javascript
for (let [car, time] of Object.entries(parkingTime)) {
}
```

일반 객체는 이터러블하지 않아 `for...of 문`을 사용할 수 없다. 그렇기 때문에 `Object.entries()` 메서드를
사용해 객체를 이터러블하도록 만든 후 `for...of 문`을 사용할 수 있도록 한다.

---

### 결과

![programmers_parking_fee_calculation_result3](/image/CodingTest/programmers_parking_fee_calculation/programmers_parking_fee_calculation_result3.png)

---

## 6. Conclusion

> 이 문제는 알고리즘 문제라긴 보단 구현 문제에 가까웠다. 정해진 조건을 얼마나 빨리 이해하고 이를
> 코드로 옮기느냐가 관건이었다. 즉, 속도가 빨라야 다음 문제도 풀 수 있을 듯 하다. 문제 자체는 어렵지 않았기 때문에
> 막히는 부분은 없었으나 처음 문제를 꼼꼼히 읽지 않아 중간에 코드를 수정하는 일이 생겼다. 앞으로는 문제 부터
> 꼼꼼히 읽어 같은 실수를 하지 않도록 하자.

---

📅 2022-09-21
