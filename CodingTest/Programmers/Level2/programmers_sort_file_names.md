# 파일명 정렬

## 1. 개요

- 프로그래머스
- Lv.2
- 2018 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/17686)

---

## 2. 문제 설명

세 차례의 코딩 테스트와 두 차례의 면접이라는 기나긴 블라인드 공채를 무사히 통과해 카카오에 입사한 무지는 파일 저장소 서버 관리를 맡게 되었다.

저장소 서버에는 프로그램의 과거 버전을 모두 담고 있어, 이름 순으로 정렬된 파일 목록은 보기가 불편했다. 파일을 이름 순으로 정렬하면 나중에 만들어진 ver-10.zip이 ver-9.zip보다 먼저 표시되기 때문이다.

버전 번호 외에도 숫자가 포함된 파일 목록은 여러 면에서 관리하기 불편했다. 예컨대 파일 목록이 ["img12.png", "img10.png", "img2.png", "img1.png"]일 경우, 일반적인 정렬은 ["img1.png", "img10.png", "img12.png", "img2.png"] 순이 되지만, 숫자 순으로 정렬된 ["img1.png", "img2.png", "img10.png", img12.png"] 순이 훨씬 자연스럽다.

무지는 단순한 문자 코드 순이 아닌, 파일명에 포함된 숫자를 반영한 정렬 기능을 저장소 관리 프로그램에 구현하기로 했다.

소스 파일 저장소에 저장된 파일명은 100 글자 이내로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.

파일명은 크게 HEAD, NUMBER, TAIL의 세 부분으로 구성된다.

- HEAD는 숫자가 아닌 문자로 이루어져 있으며, 최소한 한 글자 이상이다.
- NUMBER는 한 글자에서 최대 다섯 글자 사이의 연속된 숫자로 이루어져 있으며, 앞쪽에 0이 올 수 있다. 0부터 99999 사이의 숫자로, 00000이나 0101 등도 가능하다.
- TAIL은 그 나머지 부분으로, 여기에는 숫자가 다시 나타날 수도 있으며, 아무 글자도 없을 수 있다.

| 파일명           | HEAD | NUMBER | TAIL        |
| ---------------- | ---- | ------ | ----------- |
| foo9.txt         | foo  | 9      | .txt        |
| foo010bar020.zip | foo  | 010    | bar020.zip  |
| F-15             | F-   | 15     | (빈 문자열) |

파일명을 세 부분으로 나눈 후, 다음 기준에 따라 파일명을 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. MUZI와 muzi, MuZi는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. MUZI01.zip과 muzi1.png가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

무지를 도와 파일명 정렬 프로그램을 구현하라.

---

### 2-1. 문제 설명 - 입력 형식

입력으로 배열 files가 주어진다.

- files는 1000 개 이하의 파일명을 포함하는 문자열 배열이다.
- 각 파일명은 100 글자 이하 길이로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.
- 중복된 파일명은 없으나, 대소문자나 숫자 앞부분의 0 차이가 있는 경우는 함께 주어질 수 있다. (muzi1.txt, MUZI1.txt, muzi001.txt, muzi1.TXT는 함께 입력으로 주어질 수 있다.)

---

### 2-2. 문제 설명 - 출력 형식

위 기준에 따라 정렬된 배열을 출력한다.

---

### 2-3. 문제 설명 - 입출력 예

입력: ["img12.png", "img10.png", "img02.png", "img1.png", "IMG01.GIF", "img2.JPG"]  
출력: ["img1.png", "IMG01.GIF", "img02.png", "img2.JPG", "img10.png", "img12.png"]

입력: ["F-5 Freedom Fighter", "B-50 Superfortress", "A-10 Thunderbolt II", "F-14 Tomcat"]  
출력: ["A-10 Thunderbolt II", "B-50 Superfortress", "F-5 Freedom Fighter", "F-14 Tomcat"]

---

## 3. 문제 풀이

```javascript
function solution(files) {
  // 1) 파일 이름을 HEAD와 NUMBER로 나누기
  const separate_files = files.map((item, index) => {
    const NUMBER = item.match(/\d{1,5}/g)[0];
    const HEAD = item.split(NUMBER)[0].toLowerCase();
    return [HEAD, Number(NUMBER), index];
  });

  // 2) HEAD가 같은 파일끼리 묶은 후 NUMBER를 기준으로 정렬하기
  const map = new Map();
  separate_files.forEach(([HEAD, NUMBER, index]) => {
    const value = map.get(HEAD) || [];
    map.set(
      HEAD,
      [...value, [NUMBER, index]].sort((a, b) =>
        a[0] === b[0] ? 1 : a[0] - b[0]
      )
    );
  });

  // 3) 맵 객체의 key와 value를 바탕으로 배열을 만든 후 HEAD를 기준으로 정렬하기
  const new_files = [];
  for ([key, value] of map) {
    new_files.push([key, ...value.map((item) => item[1])]);
  }
  new_files.sort((a, b) => (a[0] > b[0] ? 1 : -1));

  // 4) 파일의 인덱스만 추출하여 새로운 배열 만들기
  const files_index = new_files.flatMap((item) => item.slice(1));

  // 5) 인덱스에 해당하는 파일을 가져와 answer 배열에 저장한 후 반환하기
  const answer = [];
  files_index.forEach((item) => {
    answer.push(files[item]);
  });

  return answer;
}
```

### 1) 파일 이름을 HEAD와 NUMBER로 나누기

```javascript
const separate_files = files.map((item, index) => {
  const NUMBER = item.match(/\d{1,5}/g)[0];
  const HEAD = item.split(NUMBER)[0].toLowerCase();
  return [HEAD, Number(NUMBER), index];
});
```

파일의 이름은 `HEAD`와 `NUMBER`로 나눈다.

파일 이름의 첫 문자부터 처음으로 숫자가 오기 전까지가 `HEAD`부분이다.  
`NUMBER`은 `HEAD`바로 이후에 등장하는 숫자(최소 한자리 최대 다섯자리)이다.

여기서 주의해야 할 점은 `HEAD`와 `NUMBER`을 제외한 영역인 `TAIL`에도
숫자가 올 수 있다는 것이다. 즉, 정규식을 이용하여 파일 이름의 모든 숫자를
가져온다면 `TAIL` 부분의 숫자도 포함되어 `NUMBER`에 대해 정렬을 할 때
문제가 생길 수 있다.

정규식에서 `\d{1,5}`의 의미를 살펴보자.

- \d: 숫자
- {min,max}: 최소 min개 이상, 최대 max개 이하

즉 최소 1개 이상 최대 5개 이하로 이루어진 숫자를 찾는 정규식이다.
만약 `TAIL`에 숫자가 있다면 2개 이상의 요소가 배열에 저장되고 그렇지 않으면
1개의 요소가 배열에 저장된다. 하지만 항상 `NUMBER`에 해당하는 값은 0번째 요소이다.

---

### 2) HEAD가 같은 파일끼리 묶은 후 NUMBER를 기준으로 정렬하기

```javascript
const map = new Map();
separate_files.forEach(([HEAD, NUMBER, index]) => {
  const value = map.get(HEAD) || [];
  map.set(
    HEAD,
    [...value, [NUMBER, index]].sort((a, b) =>
      a[0] === b[0] ? 1 : a[0] - b[0]
    )
  );
});
```

파일 이름의 `HEAD`와 `NUMBER` 그리고 파일의 순서인 `index`를 요소로 가지는
`separate_files` 배열을 순회한다. 순회를 하면서 `HEAD`은 맵 객체의 키로 만들고
`NUMBER`와 `index`는 맵 객체의 값(배열)으로 만든다.

여기서 `HEAD`는 중복이 될 수 있기 때문에 항상 이전에 있던 값을 그대로
불러와 추가하여 값을 저장해야 한다.

맵 객체의 값은 배열이며 요소는 `[NUMBER, index]` 형태의 배열이다. 이 중
0번째 요소인 `NUMBER`의 값을 기준으로 올림차순 정렬을 한다.

---

### 3) 맵 객체의 key와 value를 바탕으로 배열을 만든 후 HEAD를 기준으로 정렬하기

```javascript
const new_files = [];
for ([key, value] of map) {
  new_files.push([key, ...value.map((item) => item[1])]);
}
new_files.sort((a, b) => (a[0] > b[0] ? 1 : -1));
```

`for...of` 문을 사용하여 맵 객체를 순회한다. 각각의 `key`와 `value`를 배열의 형태로 `new_files`에
저장을 한다. 이때 `value`의 두 번째 요소인 `index`만 필요하기 때문에 `Array.map()`을 사용하여
`index`만 가져오고 `spread operator`을 사용하여 요소만 가져온다.

반복문이 끝난 후 `new_files`의 첫 번째 요소인 `HEAD`을 기준으로 정렬한다.(사전순)

---

### 4) 파일의 인덱스만 추출하여 새로운 배열 만들기

```javascript
const files_index = new_files.flatMap((item) => item.slice(1));
```

`new_files` 배열은 중첩 배열이므로 `Array.flatMap()` 메서드를 사용하여 `index`값만 가지는 새로운 배열 `files_index`를 선언한다.

---

### 5) 인덱스에 해당하는 파일을 가져와 answer 배열에 저장한 후 반환하기

```javascript
const answer = [];
files_index.forEach((item) => {
  answer.push(files[item]);
});

return answer;
```

`index`에 해당하는 파일을 `files` 배열에서 찾아 `answer` 배열에 넣는다.

---

### 결과

![programmers_sort_file_names_result1](/image/CodingTest/programmers_sort_file_names/programmers_sort_file_names_result1.png)

---

## 4. Refactoring

문제 풀이의 `2) HEAD가 같은 파일끼리 묶은 후 NUMBER를 기준으로 정렬하기` 과정에서 맵 객체가 새롭게
만들어질 때 마다 `Array.sort()` 메서드가 계속 실행된다. 하지만 정렬은 맵 객체가 모두 다 만들어 진 후
해도 괜찮다. 오히려 매번 실시하는 것 보다 한 번 실시 되는 것이 더 효율적이다.

그래서 `NUMBER`에 따른 정렬을 ` 3) 맵 객체의 key와 value를 바탕으로 배열을 만든 후 HEAD를 기준으로 정렬하기` 과정에
추가하였다.

또한 정규식에서 `g`를 삭제하여 정규식에 일치하는 첫 문자열이 있으면 더 이상 검사를 종료하게 하였다.

마지막으로 맵 객체에 값은 배열이며 이 배열의 요소 또한 배열이다. 이를 객체로 바꾸었다.

그래서 리팩토링을 마친 코드는 아래와 같다.

```javascript
function solution(files) {
  const separate_files = files.map((item, index) => {
    const NUMBER = item.match(/\d{1,5}/)[0];
    const HEAD = item.split(NUMBER)[0].toLowerCase();
    return [HEAD, Number(NUMBER), index];
  });

  const map = new Map();
  separate_files.forEach(([HEAD, NUMBER, index]) => {
    const value = map.get(HEAD) || [];
    map.set(HEAD, [...value, { number: NUMBER, index }]);
  });

  const new_files = [];
  for ([key, value] of map) {
    value.sort((a, b) => (a.number === b.number ? 1 : a.number - b.number));
    new_files.push([key, ...value.map((item) => item.index)]);
  }
  new_files.sort((a, b) => (a[0] > b[0] ? 1 : -1));

  const files_index = new_files.flatMap((item) => item.slice(1));
  const answer = [];
  files_index.forEach((item) => {
    answer.push(files[item]);
  });

  return answer;
}
```

---

### 결과

![programmers_sort_file_names_result2](/image/CodingTest/programmers_sort_file_names/programmers_sort_file_names_result2.png)

---

## 5. 다름 사람 풀이

정규식과 조건문으로 푼 문제가 인상적이여서 해당 풀이를 가져왔다.

```javascript
function solution(files) {
  const re = /^([a-zA-Z-\. ]+)([0-9]+)(.*)$/;
  let dict = [];
  files.forEach((entry, idx) => {
    let [fn, head, num] = entry.match(re);
    dict.push({ fn, head: head.toLowerCase(), num: parseInt(num), idx });
  });

  return dict
    .sort((a, b) => {
      if (a.head > b.head) return 1;
      if (a.head < b.head) return -1;
      if (a.num > b.num) return 1;
      if (a.num < b.num) return -1;
      return a.idx - b.idx;
    })
    .map((e) => e.fn);
}
```

`HEAD`, `NUMBER`, `index` 순으로 정렬을 한 조건문이 인상깊다.

---

### 결과

![programmers_sort_file_names_result3](/image/CodingTest/programmers_sort_file_names/programmers_sort_file_names_result3.png)

---

## 6. Conclusion

> 문제의 난이도는 크게 어렵지 않았다. 하지만 정렬을 할 때 사용되는 `Array.sort()` 메서드에서 숫자와
> 문자를 비교할 때의 차이에 대해 정확히 모르고 있었기 때문에 해당 부분에서 시간을 조금 소비하였다. 해당 차이에
> 대해 잘 숙지하고 있었다면 풀이 시간은 20~30분 정도 짧아졌을 것이라 생각이 되어 아쉽다. 그리고 생각보다 정규식이
> 필요한 문제가 많이 보인다. 내가 원하는 정규식을 잘 사용할 수 있도록 공부를 해봐야겠다.

---

📅 2022-09-11
