# 스킬트리

## 1. 개요

- 프로그래머스
- Lv.2
- Summer/Winter Coding(~2018)
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/49993#fnref1)

---

## 2. 문제 설명

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 스파크 → 라이트닝 볼트 → 썬더일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 스파크 → 힐링 → 라이트닝 볼트 → 썬더와 같은 스킬트리는 가능하지만, 썬더 → 스파크나 라이트닝 볼트 → 스파크 → 힐링 → 썬더와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

### 2-1. 문제 설명 - 제한 조건

- 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 스킬 순서와 스킬트리는 문자열로 표기합니다.
  - 예를 들어, C → B → D 라면 "CBD"로 표기합니다
- 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
- skill_trees는 길이 1 이상 20 이하인 배열입니다.
- skill_trees의 원소는 스킬을 나타내는 문자열입니다.
  - skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

### 2-2. 문제 설명 - 입출력 예

| skill | skill_trees                       | return |
| ----- | --------------------------------- | ------ |
| "CBD" | ["BACDE", "CBADF", "AECB", "BDA"] | 2      |

### 2-3. 문제 설명 - 입출력 예 설명

- "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
- "CBADF": 가능한 스킬트리입니다.
- "AECB": 가능한 스킬트리입니다.
- "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.

---

## 3. 문제 풀이

```javascript
function solution(skill, skill_trees) {
  // 1) 유저가 배울 수 있는 스킬 순서
  const possibleSkillTress = [''];

  // 2) 유저가 배울 수 있는 스킬 순서 구하기
  [...skill].reduce((acc, cur) => {
    possibleSkillTress.push(acc + cur);
    return acc + cur;
  }, '');

  // 3) 유저들이 만든 스킬트리 중 선행 스킬 순서에 포함된 스킬만 가져오기
  skill_trees = skill_trees.map((tree) => {
    return [...tree].filter((item) => [...skill].includes(item)).join('');
  });

  // 4) 유저가 배울 수 있는 스킬 순서에 포함된 유저들이 만든 스킬트리의 길이 반환하기
  return skill_trees.filter((tree) => possibleSkillTress.includes(tree)).length;
}
```

### 1) 유저가 배울 수 있는 스킬 순서

```javascript
const possibleSkillTress = [''];
```

`possibleSkillTress` 변수에는 유저들이 배울 수 있는 스킬 순서가 담기는데, 이는 `skill` 문자열을 바탕으로 만들어진다. `""` 빈 문자열이 있는 이유는 어떠한 스킬을 배우지 않는 것 또한 유저가 배울 수 있는 스킬이기 때문이다.

### 2) 유저가 배울 수 있는 스킬 순서 구하기

```javascript
[...skill].reduce((acc, cur) => {
  possibleSkillTress.push(acc + cur);
  return acc + cur;
}, '');
```

유저들은 선행 스킬 순서를 따라 스킬을 배울 수 있다. 이말은 특정 스킬을 배우기 전에 선행 스킬을 반드시 배워야 한다는 것이다. 이를 구현하기 위해 `reduce` 메서드를 사용하여 `skill` 문자열을 처음부터 순서대로 더한 값들을 `possibleSkillTress` 배열에 추가한다.

### 3) 유저들이 만든 스킬트리 중 선행 스킬 순서에 포함된 스킬만 가져오기

```javascript
skill_trees = skill_trees.map((tree) => {
  return [...tree].filter((item) => [...skill].includes(item)).join('');
});
```

`skill_trees` 배열에는 유저들이 만든 스킬트리들이 요소로 존재한다. 각각의 스킬트리에서 선행 스킬 순서에 포함된 스킬만 가져온다. 선행 스킬 순서에 포함되지 않은 스킬을 비교하는 것은 무의미 하기 때문이다. 예를들어 선행 스킬 순서가 "BC"라고 할 때, "ABCE"는 "AB"로 "BDE"는 "B"로 바뀌게 된다.

이때, 만약 어떤 스킬트리에 어떠한 선행 스킬 순서에 포함된 스킬이 없다면 빈 문자열을 반환한다.

### 4) 유저가 배울 수 있는 스킬 순서에 포함된 유저들이 만든 스킬트리의 길이 반환하기

```javascript
return skill_trees.filter((tree) => possibleSkillTress.includes(tree)).length;
```

`skill_trees`의 배열의 요소를 `possibleSkillTress` 배열에 포함된 여부를 비교한다. 만약 `possibleSkillTress` 배열에 `skill_tress`의 어떤 요소가 포함된다면 이는 습득 가능한 스킬이라고 할 수 있다. 해당 배열의 길이를 반환한다.

### 결과

![programmers_skill_trees_result](/image/CodingTest/programmers_skill_trees/programmers_skill_trees_result.png)

---

## 4. 다른 사람의 풀이 1

정규 표현식을 사용한 풀이를 가져왔다. 좋아요도 많이 달린 풀이이다.

```javascript
function solution(skill, skill_trees) {
  var answer = 0;
  var regex = new RegExp(`[^${skill}]`, 'g');

  return skill_trees
    .map((x) => x.replace(regex, ''))
    .filter((x) => {
      return skill.indexOf(x) === 0 || x === '';
    }).length;
}
```

정규 표현식부터 살펴보자.

`g` 플래그로 패턴에 일치하는 모든 문자를 매칭한다. `[^${skill}]` 이는 변수를 정규 표현식 내부에서 사용하기 위한 표현이다. 해당 패턴을 파악해보자.

- []: 내부 패턴에 OR이 적용된다.
- []내부의 ^: 패턴의 반대가 적용된다.

즉, `skill` 문자열의 각각 문자가 아닌 것들과 매칭된다.

그 다음 `map` 메서드를 통해 정규 표현식과 매칭되는 것들을 ""로 치환한다. 이로써 `skill` 문자열에 있는 스킬들이 아닌 것들은 제거된다.

마지막으로 `filter` 메서드를 통해 스킬 순서가 올바른 스킬 트리들만 거른다. 이후 해당 배열의 길이를 반환한다.

### 결과

![programmers_skill_trees_result2](/image/CodingTest/programmers_skill_trees/programmers_skill_trees_result2.png)

---

## 5. 다른 사람의 풀이 2

```javascript
function solution(skill, skill_trees) {
  function isCorrect(n) {
    let test = skill.split('');
    for (var i = 0; i < n.length; i++) {
      if (!skill.includes(n[i])) continue;
      if (n[i] === test.shift()) continue;
      return false;
    }
    return true;
  }

  return skill_trees.filter(isCorrect).length;
}
```

`skill_trees` 배열의 요소를 `isCorrect` 함수 내에서 검사하는 것을 기본으로 하는 코드로 보인다.

`isCorrect` 함수 내에서는 인자로 받은 `skill_trees` 배열의 요소 즉, 유저의 스킬 트리를 `for...문`을 통해 검사를 한다. 스킬 트리의 처음 부터 비교하기 시작하는데, 아래의 조건에 따라 반복문이 계속될지 `false`를 반환할지 결정된다.

1. 선행 스킬 순서에 스킬이 포함되지 않으면 다음 반복문으로 넘어간다.
2. 선행 스킬 순서에 스킬이 포함되고 `test` 변수의 첫 번째 요소라면 다음 반복문으로 넘어간다.

위의 조건을 만족하지 않으면 `false`를 반환한다. 그리고 반복문이 성공적으로 종료되면 `true`를 반환한다.

### 결과

![programmers_skill_trees_result2](/image/CodingTest/programmers_skill_trees/programmers_skill_trees_result3.png)

---

## 6. Conclusion

> 역시 다양한 접근법이 존재한다. 난 과정을 나누어 차례차례 문제를 풀었다면 다른 사람의 풀이를 보면 모든 과정을 한 번에 묶어 풀었다. 최근에 정규 표현식을 정리하였다. 그래서 해당 문제를 정규 표현식을 사용하여 풀려하였지만 적당한 풀이가 생각나지 않아 정규 표현식을 사용하지 못하였다. `skill`에 존재하는 스킬을 포함하는 것에만 집중을 해서 그런 듯 하다. 부정도 유심히 살피자.

---

📅 2023-01-31
