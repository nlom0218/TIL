# Style Sheet

# 0. 바로가기

- [1. Inline Style](#1-inline-style인라인-스타일)
- [2. Internal Style Sheet](#2-internal-style-sheet내부-스타일-시트-embedding-style)
- [3. Linking Style Sheet](#3-linking-style-sheet외부-스타일-시트-link-style)

---

## 1. Inline Style(인라인 스타일)

간단한 스타일 정보를 스타일 시트를 사용하지 않고 스타일을 적용할 대상에 직접 표시하는 방법  
style="속성: 속성값;"형태로 스타일을 바꿈

```html
<p style="color: blue">Lorem ipsum dolor.</p>
```

- 단점
  - 꾸미는 데 한계가 있음
  - 재사용이 불가능

---

## 2. Internal Style Sheet(내부 스타일 시트), Embedding style

웹 문서 안에서 사용할 스타일을 같은 문서 안에 정리한 것  
`<head>`태그 안에서 정의하고 `<style>과 <style>`태그 사이에 작성

```html
...
<head>
  ...
  <style>
    h1 {
      font-size: 20px;
      background-color: red;
      color: #fff;
    }
  </style>
  ...
</head>
<body>
  <h1>Welcome!</h1>
</body>
```

- 장점

  - HTML 문서 안 여러 요소(태그)를 한번에 꾸밀 수 있음

- 단점
  - 또 다른 HTML 문세어는 적용할 수 없음

---

## 3. Linking Style Sheet(외부 스타일 시트), Link style

여러 웹 문서에서 사용할 스타일을 별도 파일로 저장해 놓고 필요할 때 마다 가져와서 사용  
`*.css`라는 파일 확장자를 사용

```html
...
<head>
  ...
  <link rel="stylesheet" href="css/style.css" />
  ...
</head>
<body>
  <h1>Welcome!</h1>
</body>
```

```css
/* css/style.css */
h1 {
  font-size: 20px;
  background-color: red;
  color: #fff;
}
```

- 장점
  - 여러 HTML문서에 사용할 수 있음

---

[Next - Selector](/CSS/Selector.md)
