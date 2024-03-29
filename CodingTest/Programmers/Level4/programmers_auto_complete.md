# 자동완성

## 1. 개요

- 프로그래머스
- Lv.4
- 2018 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/17685)

---

## 2. 문제 설명

포털 다음에서 검색어 자동완성 기능을 넣고 싶은 라이언은 한 번 입력된 문자열을 학습해서 다음 입력 때 활용하고 싶어 졌다. 예를 들어, go 가 한 번 입력되었다면, 다음 사용자는 g 만 입력해도 go를 추천해주므로 o를 입력할 필요가 없어진다! 단, 학습에 사용된 단어들 중 앞부분이 같은 경우에는 어쩔 수 없이 다른 문자가 나올 때까지 입력을 해야 한다.
효과가 얼마나 좋을지 알고 싶은 라이언은 학습된 단어들을 찾을 때 몇 글자를 입력해야 하는지 궁금해졌다.

예를 들어, 학습된 단어들이 아래와 같을 때

> go  
> gone  
> guild

- go를 찾을 때 go를 모두 입력해야 한다.
- gone을 찾을 때 gon 까지 입력해야 한다. (gon이 입력되기 전까지는 go 인지 gone인지 확신할 수 없다.)
- guild를 찾을 때는 gu 까지만 입력하면 guild가 완성된다.

이 경우 총 입력해야 할 문자의 수는 7이다.

라이언을 도와 위와 같이 문자열이 입력으로 주어지면 학습을 시킨 후, 학습된 단어들을 순서대로 찾을 때 몇 개의 문자를 입력하면 되는지 계산하는 프로그램을 만들어보자.

---

### 2-1. 문제 설명 - 입력 형식

학습과 검색에 사용될 중복 없는 단어 N개가 주어진다.  
모든 단어는 알파벳 소문자로 구성되며 단어의 수 N과 단어들의 길이의 총합 L의 범위는 다음과 같다.

- 2 <= N <= 100,000
- 2 <= L <= 1,000,000

---

### 2-2. 문제 설명 - 출력 형식

단어를 찾을 때 입력해야 할 총 문자수를 리턴한다.

---

### 2-3. 입출력 예제

| words                            | result |
| -------------------------------- | ------ |
| ["go","gone","guild"]            | 7      |
| ["abc","def","ghi","jklm"]       | 4      |
| ["word","war","warrior","world"] | 15     |

---

### 2-4. 입출력 설명

- 첫 번째 예제는 본문 설명과 같다.
- 두 번째 예제에서는 모든 단어들이 공통된 부분이 없으므로, 가장 앞글자만 입력하면 된다.
- 세 번째 예제는 총 15 자를 입력해야 하고 설명은 아래와 같다.
  - word는 word모두 입력해야 한다.
  - war는 war 까지 모두 입력해야 한다.
  - warrior는 warr 까지만 입력하면 된다.
  - world는 worl까지 입력해야 한다. (word와 구분되어야 함을 명심하자)

---

## 3. 문제 풀이

```javascript
class Node {
  constructor(value = "") {
    this.value = value;
    this.children = new Map();
    this.is_word = false;
  }
}

class Trie {
  constructor() {
    this.root = new Node();
  }

  insert(string) {
    let curNode = this.root;

    for (const char of string) {
      if (!curNode.children.has(char)) {
        curNode.children.set(char, new Node(curNode.value + char));
      }
      curNode = curNode.children.get(char);
    }
    curNode.is_word = true;
  }

  // 1) 문자의 마지막 노드 가져오기
  last_node(string) {
    let curNode = this.root;
    for (const char of string) {
      curNode = curNode.children.get(char);
    }
    return curNode;
  }

  // 2) 문자가 포함된 모든 단어 파악하기
  includes_words(string) {
    let curNode = this.root;
    let arr = [];
    for (const char of string) {
      curNode = curNode.children.get(char);
      if (curNode.is_word) arr.push(curNode.value);
    }
    return arr;
  }

  // 3) 단어가 검색되기 위해 필요한 최소 입력 수 구하기
  min_len(string) {
    if (this.last_node(string).children.size !== 0) return string.length;

    let curNode = this.root;
    const path = [];
    for (const char of string) {
      curNode = curNode.children.get(char);
      path.push(curNode.children.size);
    }
    path.pop();
    while (path[path.length - 1] === 1) {
      path.pop();
    }

    let words = this.includes_words(string);
    if (words.length === 1) return path.length + 1;
    if (path.length + 1 < words[words.length - 2].length + 1)
      return words[words.length - 2].length + 1;
    return path.length + 1;
  }
}

function solution(words) {
  const trie = new Trie();
  // 4) 트라이 자료구조에 단어 삽입하기
  words.forEach((item) => trie.insert(item));

  // 5) 각각의 단어들이 검색되기 위한 최소 입력 수를 담은 배열 만들기
  const min_len = words.map((item) => trie.min_len(item));

  // 6) 모든 요소의 합을 반환하기
  return min_len.reduce((acc, cur) => acc + cur);
}
```

트라이 자료구조를 자바스크립트로 구현하는 기초적인 과정은 아래의 링크에서 확인가능하다. 하지만 기본 틀에서 벗어나
해당 문제를 풀기 위해 추가적으로 작성한 코드가 있다. 해당 코드를 위주로 설명을 진행한다.

[트라이 자료구조 파트 바로가기](DataStructureAlgorithm/DataStructure/Trie.md)

---

### 1) 문자의 마지막 노드 가져오기

```javascript
last_node(string) {
  let curNode = this.root;
  for (const char of string) {
    curNode = curNode.children.get(char);
  }
  return curNode;
}
```

매개변수로 들어온 문자의 마지막 노드를 가져오는 함수이다.

루트 노드부터 시작하여 문자의 첫 번째 부터 반복문을 통해 마지막 노드까지 도달하여 마지막 노드를 찾는다.

---

### 2) 문자가 포함된 모든 단어 파악하기

```javascript
includes_words(string) {
  let curNode = this.root;
  let arr = [];
  for (const char of string) {
    curNode = curNode.children.get(char);
    if (curNode.is_word) arr.push(curNode.value);
  }
  return arr;
}
```

매개변수로 받은 문자가 포함된 모둔 단어를 파악하는 함수이다. 예를 들어 "hello"라는 단어가 들어왔을 때
마지막 노드까지 도달하는 과정에서 발견되는 단어를 리턴한다.

단어는 `insert` 함수로 트라이 자료구조에 삽입한 것이며 예를 들어 "hell"이라는 단어를 삽입했을 때 해당
값을 가지는 노드는 단어이지만 "hel"은 단어가 아니다.

루트 노드부터 시작하여 매개변수로 받은 문자의 첫 번째 문자열 부터 반복문을 통해 마지막 노드까지 이동하는 과정에서
각각의 노드 중 `is_word`의 값이 `ture`인 노드만 배열에 담아 반환한다.

---

### 3) 단어가 검색되기 위해 필요한 최소 입력 수 구하기

```javascript
min_len(string) {
  if (this.last_node(string).children.size !== 0) return string.length;

  let curNode = this.root;
  const path = [];
  for (const char of string) {
    curNode = curNode.children.get(char);
    path.push(curNode.children.size);
  }
  path.pop();
  while (path[path.length - 1] === 1) {
    path.pop();
  }

  let words = this.includes_words(string);
  if (words.length === 1) return path.length + 1;
  if (path.length + 1 < words[words.length - 2].length + 1)
    return words[words.length - 2].length + 1;
  return path.length + 1;
}
```

매개변수 받은 단어가 검색되기 위한 최소한의 입력 수를 구하는 함수이다. 단, 다른 단어는 검색이 되어서는
안되면 오로지 하나의 단어만 검색이 되어야 한다.

전체적인 로직은 아래와 같다.

- 만약 단어의 마지막 노드의 `children`이 있다면 그 이후에 단어가 최소 1개 이상 더 있는 것이기 때문에
  단어의 길이를 반환한다.
- 그렇지 않다면 해당 단어의 마지막 노드까지 가면서 각각의 노드가 가지고 있는 `children`의 수를 `path` 배열에
  넣는다.
- `children`은 해당 노드의 간선이고 만약 간선이 2개 이상이라면 또 다른 단어로 이어진다.
- `path` 배열의 마지막 요소가 `1`이 될 때까지 `Array.pop()` 메서드를 사용한다.
- 요소가 `1`이라는 것은 단어로 이어지는 경로가 단 하나 뿐이기 때문에 입력을 하지 않아도 되는 문자열이 된다.
- 그 후 `includes_words` 함수를 통해 매개변수로 받은 단어까지 입력하면서 어떤 단어들이 있는지 검사한다.
- 만약 포함된 단어가 하나라면 바로 `path` 배열 길이에 1을 더한 값을 반환한다.
- 만약 포함된 단어가 둘 이상이라면 해당 단어까지 가는 경로 중 또 다른 단어가 포함되어 있다는 것이다. 예를 들어
  `war`와 `warrior` 단어가 있다고 하자. `warrior` 단어까지 가는 경로 중 `war` 단어가 있기 때문에 `w`, `wa`,
  `war`를 입력해서는 `warrior`만 검색 결과에 나타나지 않는다.
- 그렇기 때문에 마지막 전 요소의 문자의 길이에 1을 더한 값을 반환한다.
- 그 외의 상황에는 `path` 배열 길이에 1을 더한 값을 반환한다.

> 지금 다시 봐도 넘나 복잡하다ㅠㅠ 아래의 리팩토링을 통해 구현한 코드를 보면 좀 더 직관적이고 쉽다.

---

### 4) 트라이 자료구조에 단어 삽입하기

```javascript
words.forEach((item) => trie.insert(item));
```

`Array.forEach()` 메서드를 사용하여 `words` 요소를 순회하며 트라이 자료구조에 삽입한다.

---

### 5) 각각의 단어들이 검색되기 위한 최소 입력 수를 담은 배열 만들기

```javascript
const min_len = words.map((item) => trie.min_len(item));
```

각각의 단어들만 검색되는 최소 입력 수를 구한 배열을 만든다.

---

### 6) 모든 요소의 합을 반환하기

```javascript
return min_len.reduce((acc, cur) => acc + cur);
```

`Array.reduce()` 메서드를 사용하여 모든 요소의 합을 구해 반환하다.

> 자료 구조를 구현하는 것이 길고 어려웠을 뿐 이를 이용하여 값을 찾는 과정은 쉽다. 자바스크립트의 내장 함수
> 확장을 격렬히 원한다.

---

### 결과

![programmers_auto_complete_result1](/image/CodingTest/programmers_auto_complete/programmers_auto_complete_result1.png)

---

## 4. Refactoring

트라이 자료 구조의 구현을 조금 더 간단하게 작성하였다.

```javascript
class Node {
  constructor(value = "", end = false) {
    this.value = value;
    this.end = end;
    this.child = new Map();
    this.includesWords = [];
  }
}

class Trie {
  constructor() {
    this.root = new Node();
  }
  insert(string) {
    let cur_node = this.root;

    for (const char of string) {
      if (!cur_node.child.has(char)) {
        cur_node.child.set(char, new Node(cur_node.value + char));
      }
      cur_node = cur_node.child.get(char);
      cur_node.includesWords.push(string);
    }
    cur_node.end = true;
  }
  min_len(string) {
    let cur_node = this.root;
    let len = 0;
    for (const char of string) {
      cur_node = cur_node.child.get(char);
      len++;
      if (cur_node.includesWords.length === 1) return len;
    }
    return len;
  }
}

function solution(words) {
  const trie = new Trie();
  words.forEach((item) => trie.insert(item));
  return words.map((item) => trie.min_len(item)).reduce((a, b) => a + b);
}
```

위 `Trie Class`는 [트라이 자료구조 파트 바로가기](DataStructureAlgorithm/DataStructure/Trie.md)에서
자세히 살펴볼 수 있다.

---

### 결과

![programmers_auto_complete_result2](/image/CodingTest/programmers_auto_complete/programmers_auto_complete_result2.png)

---

## 5. 코뎅테스트 광탈 방지 풀이

```javascript
function makeTrie(words) {
  // 1) 루트 노드를 설정할 변수 만들기
  const root = {};
  // 2) Trie를 구성하기 위한 반복문 실행하기
  for (const word of words) {
    let current = root;
    for (const letter of word) {
      // 3) 단어의 글자를 하나씩 추출 한 후 해당 값이 없으면 새로운 리스트를 만들기
      if (!current[letter]) current[letter] = [1, {}];
      // 4) 값이 있으면 기존 리스트의 첫 번째 값을 카운팅을 위해 1을 더하기
      else current[letter][0] = 1 + current[letter][0];
      // 5) current는 letter에 해당되는 노드로 이동하기
      current = current[letter][1];
    }
  }

  return root;
}

function solution(words) {
  let answer = 0;
  const trie = makeTrie(words);

  for (const word of words) {
    let count = 0;
    let current = trie;
    for (const letter of word) {
      // 6) 현재 노드의 추출한 단어의 값에 따라 반복문 실행하기
      count += 1;
      if (current[letter][0] <= 1) break;
      current = current[letter][1];
    }
    answer += count;
  }

  return answer; // 반환
}
```

---

### 1) 루트 노드를 설정할 변수 만들기

```javascript
const root = {};
```

위의 풀이에서는 노드를 객체로 관리한다. 그리고 초기 루트 노드를 위해 빈 객체를 만든다.

---

### 2) Trie를 구성하기 위한 반복문 실행하기

```javascript
for (const word of words) {
  let current = root;
  for (const letter of word) {
  }
}
```

배열의 각 요소(문자열)를 반복문을 통해 트라이 자료 구조를 구현한다. 반복문에서 항상 첫 시작은 루트 노드가 된다.
또한 이중 반복문을 사용하고 있으면 두 번째 반복문에서는 단어의 글자를 하나씩 추출하여 반복문을 실행한다.

---

### 3) 단어의 글자를 하나씩 추출 한 후 해당 값이 없으면 새로운 리스트를 만들기

```javascript
if (!current[letter]) current[letter] = [1, {}];
```

만약 현재 노드에 추출 한 글자에 해당하는 값이 없으면 새로운 리스트를 만든다. 리스트의 첫 번째 값은
학습된 단어가 몇 개인지를 카운팅하고 두 번째 값은 트리 구조로 이용할 노드 값으로 사용한다.

---

### 4) 값이 있으면 기존 리스트의 첫 번째 값을 카운팅을 위해 1을 더하기

```javascript
current[letter][0] = 1 + current[letter][0];
```

만약 현재 노드에 추출 한 글자에 해당하는 값이 있으면 기존의 리스트에 첫 번째 값만 1을 더한다.

---

### 5) current는 letter에 해당되는 노드로 이동하기

```javascript
current = current[letter][1];
```

이후 `current`를 추출 한 글자에 해당하는 값으로 바꾼다.

---

### 6) 현재 노드의 추출한 단어의 값에 따라 반복문 실행하기

```javascript
count += 1;
if (current[letter][0] <= 1) break;
current = current[letter][1];
```

`count`는 단어를 찾기 위해 입력하는 횟 수 이다.

만약 현재 노드의 `letter`에 대한 값중 첫 번째 요소가 1보다 작다면 즉, 단어가 하나 이하로 남을 경우
반복문을 종료한다. 단어가 하나 이하로 남는다는 것은 해당 횟 수 까지 입력을 했을 때 단 하나의 단어만
검색이 된다는 뜻이다.

그렇지 않다면`current`를 `letter`헤 대한 값중 두 번째 요소로 바꾼다.

이런 과정은 내가 리팩토링 한 풀이에서 `min_len`를 구할 때의 과정과 같다.

---

### 결과

![programmers_auto_complete_result3](/image/CodingTest/programmers_auto_complete/programmers_auto_complete_result3.png)

---

## 6. Conclusion

> 강의 풀이를 공부하면서 느꼈던 점은 실제 코딩 테스트에서 더욱 간단한 풀이를 찾는 것이 중요하다고 생각했다.
> 내가 구현 한 방법도 내가 느끼기엔 나쁘지 않았지만 일단 코드가 길어 생각하는 것이 많아지기 때문에 그 만큼
> 시간이 길어져 촉박한 마음이 들 것 같다. 여러 방법에 대해 생각하고 문제에 적합한 구조를 잘 나타내야 하는
> 연습을 많이 해봐야겠다.

---

## 참고

코딩테스트 광탈 방지 A to Z : Javascript Ch11-3.

---

📅 2022-09-16
