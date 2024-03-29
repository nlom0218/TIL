# 특정 문자 숫자 매칭 패턴

## 0. 요약

| 기호       | 의미                                                                |
| ---------- | ------------------------------------------------------------------- |
| a-zA-Z     | 영어알파벳(-으로 범위 지정)                                         |
| ㄱ-ㅎ가-힣 | 한글 문자(-으로 범위 지정)                                          |
| 0-9        | 숫자(-으로 범위 지정)                                               |
| .          | 모든 문자(숫자, 한글, 영어, 특수기호, 공백 모두), 줄바꿈(\n)은 제외 |
| \d         | 숫자                                                                |
| \D         | 숫자가 아닌 것                                                      |
| \w         | 밑줄 문자를 포함한 영숫자 문자에 대응                               |
| \W         | \w 가 아닌 것                                                       |
| \s         | space 공백                                                          |
| \S         | space 공백이 아닌 것                                                |
| \특수기호  | 특수기호 \* \^ \& \! \? ...등                                       |
| \b         | 63개 문자가 아닌 나머지 문자에 일치하는 경계                        |
| \B         | 63개 문자에 일치하는 경계                                           |

---

## 1. a-zA-Z 패턴

`a-zA-Z` 패턴은 영어 알파벳을 찾을 때 사용한다. 단, 괄호([])안에서 사용을 해야한다. 왜냐 하면 괄호가 없다면 `a-zA-Z`라는 문자열 그 자체를 의미하기 때문이다.(아래 사진 참고)

![a-zA-Z 패턴](/image/RegExp/regExpPattern11.png)

## 1-1. a-zA-Z 패턴 사용예시

정규 표현식은 `/[a-zA-Z]/`이다. 이는 `/[a-z]/i`, `/[A-Z]/i` 와 같다. 문자열 중 영어 알파벳을 찾을 수 있다. 모든 영어 알파벳을 찾기 위해선 `g` 플래그가 있어야 하지만 예시에선 없기 때문에 가장 처음의 영어 알파벳이 반환된다.

![a-zA-Z 패턴](/image/RegExp/regExpPattern12.png)

---

## 2. ㄱ-ㅎ가-힣 패턴

`ㄱ-ㅎ가-힣` 패턴은 한글 문자를 찾을 때 사용한다. 해당 패턴도 괄호([])안에서 사용을 해야한다.

### 2-1. ㄱ-ㅎ가-힣 패턴 사용예시

정규 표현식 `ㄱ-ㅎ가-힣` 패턴의 간단한 사용 예시이다.

![ㄱ-ㅎ가-힣 패턴](/image/RegExp/regExpPattern13.png)

---

## 3. 0-9 패턴

`0-9` 패턴은 숫자를 찾을 때 사용한다. 해당 패턴도 괄호([])안에서 사용을 해야한다.

### 3-1. 0-9 패턴 사용예시

정규 표현식 `0-9` 패턴의 간단한 사용 예시이다.

![0-9 패턴](/image/RegExp/regExpPattern14.png)

---

## 4. . 패턴

`.` 패턴은 모든 문자을 찾을 때 사용한다. 단 줄바꿈(\n)은 제외한다. 줄바꿈까지 포함시키기 위해선 `s` 플래그가 필요하다.

또한 `.` 패턴은 괄호([])안에서는 사용이 불가능하다. 개인적으로 테스트를 해본 결과 괄호([])안에서 사용하게 되면 `.` 패턴은 모든 문자가 아니라 `.` 문자 그대로를 인식한다.

### 4-1. . 패턴 사용예시 1

정규 표현식은 `/./`이다.

![. 패턴](/image/RegExp/regExpPattern15.png)

가장 첫 문자인 `안`도 모든 문자에 포함함으로 바로 매칭이 된다.

### 4-2. . 패턴 사용예시 2

정규 표현식은 `/.../`이다. 해당 정규 표현식은 3개의 어떠한 문자로 이루어진 문자열을 뜻한다. 그러므로 `안녕!`이 매칭된다.

![. 패턴](/image/RegExp/regExpPattern16.png)

### 4-3. . 패턴 사용예시 3

정규 표현식은 `/e.t/`이다. 해당 정규식은 `m`과 `t` 사이에 어떠한 문자도 들어간 문자열을 뜻한다. 때문에 예시에서는 `e t`가 매칭된 것을 볼 수 있다. 공백도 모든 문자에 포함된다.

![. 패턴](/image/RegExp/regExpPattern17.png)

`g` 플래그를 사용하면 `met`도 함께 매칭된다.

![. 패턴](/image/RegExp/regExpPattern18.png)

---

## 5. \d 패턴과 \D 패턴

`\d`는 숫자 1개를 의미하고 `\D`는 그 반대로 숫자가 아닌 문자 1개를 의미한다.

괄호안에서 쓰일 경우 `/[\d]/`는 `/[0-9]/`와 같고 `/[\D]/`는 `/[^0-9]/`와 같다.

### 5-1. \d 패턴과 \D 패턴 사용예시

정규 표현식 `/\d/`와 `/\D/`의 간단한 사용 예시이다.

`/\d/`인 경우, 문자열에서 가장 처음으로 등장하는 숫자 1개를 탐색하고 `/\D/`인 경우에는 문자열에서 가장 처음으로 등장하는 숫자가 아닌 문자 1개를 탐색한다.

![\d 패턴](/image/RegExp/regExpPattern19.png)
![\D 패턴](/image/RegExp/regExpPattern20.png)

---

## 6. \w 패턴과 \W 패턴

`\w` 패턴은 밑줄 문자를 포함한 영숫자 문자를 의미하며 `\W`는 `\w`를 제외한 것을 의미한다.

정규 표현식 `/[\w]/`은 `/[A-Za-z0-9_]/`와 동일하다.

### 6-1. \w 패턴과 \W 패턴 사용예시

정규 표현식 `/\w/` 패턴과 `\W` 패턴의 간단한 사용 예시이다.

`/\w/`인 경우, 밑줄 문자를 포함한 영숫자 문자인 `N`이 탐색되고 `/\W/`인 경우에는 `안`이 탐색된다.

![\w 패턴](/image/RegExp/regExpPattern23.png)
![\W 패턴](/image/RegExp/regExpPattern24.png)

---

## 7. \특수기호

`\특수기호` 패턴은 특수기호(\*, ^, &, !, ?)등을 문법 키워드가 아닌 일반 문자로 인식되록 한다. 예를 들어 ^은 괄호([]) 밖에서는 문자의 시작, 괄호([])안에서는 부정을 의미하는데 앞에 `\`를 붙임으로써 `^` 문자 그대로를 뜻하게 만든다.

### 7-1. \특수기호 사용예시

정규 표현식 `/[^\w]/`은 밑줄 문자를 포함한 영숫자 문자를 제외한 문자에 대응을 하지만 정규 표현식 `/[\^\w]/`은 밑줄 문자를 포함한 영숫자 문자 또는 `^`기호에 대응한다.

아래에서 두 정규 표현식의 차이를 찾을 수 있다.

![\특수기호 사용예시](/image/RegExp/regExpPattern21.png)

위 사진에선 밑줄 문자를 포함한 영숫자 문자를 제외한 문자인 `안`이 탐색되었다.

![\특수기호 사용예시](/image/RegExp/regExpPattern22.png)

위 사진에서 밑줄 문자를 포함한 영숫자 문자 또는 `^`기호 중 `N`이 탐색되었다.

---

## 8. \b 패턴

`\b`는 단어의 경계 위치를 가리킨다. 여기서 단어는 `\w`와 일치하며 `[a-zA-Z0-9_]`와 동일하다. 즉, `단어`와 `단어가 아닌 문자`와의 사이를 가리킨다.

쉽게 생각하자면 `\b` 패턴은 찾는 패턴이 단어의 처음 또는 긑에 있는지 찾는다.

하나의 간단한 예를 들어보자.

내가 원하는 결과는 `to` 단어를 찾는 것이다. 때문에 정규 표현식을 `/to/g`하여 결과를 보니 `to`뿐 만 아니라 `to`가 포함된 단어도 찾아졌다.

![\b 패턴 사용예시](/image/RegExp/regExpPattern9.png)

내가 원하는 결과가 아니다. 난 순수히 `to` 단어만 찾고 싶다. 이럴때 유용하게 사용할 수 있는 패턴이 바로 `\b`이다.

`공백`은 `단어가 아닌 문자`이다. 그렇기 때문에 내가 찾고자 하는 패턴의 양 옆에 `\b` 패턴을 사용하면 순수한 `to` 단어를 찾을 수 있다.

### 8-1. \b 패턴 사용예시

정규 표현식은 `/\bto\b/g`이다.

- /\bto\b/: `단어가 아닌 문자` + to + `단어가 아닌 문자`의 경우를 찾는다.
- g: 문자열 내 모든 패턴을 검색한다. 결과가 하나 이상일 수 있다.

![\b 패턴 사용예시](/image/RegExp/regExpPattern10.png)

---

## 9. \B 패턴

`\B` 패턴은 `\b` 패턴과 반대로 동작한다. 즉, 찾는 패턴이 단어의 처음이 아니거나 끝이 아닌 경우를 찾는다. 단어의 중간에 숨은 패턴을 찾을 때 사용한다고 생각하면 된다.

---

## 10. Conclusion

> 정규 표현식의 많은 패턴을 알아보았다. 이번에 정리한 내용이 단독으로 쓰이기도 하고 이전에 정리한 검색 기준 패턴과 함께 사용하기도 한다. 뿐만 아니라 앞으로 정리하게 될 정규식 갯수 반복 패턴과도 함께 쓰이니 개별의 개념으로 이해하지 말고 전체적으로 하나의 개념으로 이해하도록 하자.  
> 여러 예시를 통해 학습을 하고 있는데, 모든 정규식 패턴을 정리한 후 보다 복잡한 정규식 패턴을 정리를 해보며 지식을 쌓아야 겠다는 생각이 들었다. 또한 자주 사용하는 정규식 패턴을 따로 정리해 둘 필요도 있을 듯 하다.

---

## 참고

[[JS] 📚 정규표현식(RegExp) - 이해하기 쉽게 정리 + 응용 예제](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)  
[정규식 단어 경계 메타 문자 \b 의 정확한 이해](https://ohgyun.com/392)  
[정규표현식(Regular Expression, regex)](https://blog.walkinpcm.com/15)

---

📅 2023-01-24
