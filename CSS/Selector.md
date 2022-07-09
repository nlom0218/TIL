# Seletor

## 👉 바로가기

- [1. 개요](#1-개요)
- [2. 전체 셀럭터(Universal Selector)](#2-전체-셀렉터universal-selector)
- [3. 태그/타입 셀렉터(Type Selector)](#3-태그타입-셀렉터type-selector)
- [4. ID 셀렉터(ID Selector)](#4-id-셀렉터id-selector)
- [5. 클래스 셀렉터(Class Selector)](#5-클래스-셀렉터class-selector)

---

## 1. 개요

style를 적용 하고자하는 HTML요소를 특정할 필요가 있다. 이때 사용하는 것이 셀렉터/선택자(Selector)이다. style를 적용하고자하는 HTML 요소를 셀렉터로 특정하고 선택된 요소에 스타일을 정의하는 것. 복수개의 셀렉터를 연속하여 지정할 수 있으며 쉼표(,)로 구분함.
![CSS Rule Set](../image/CSS/CSSRuleSet.png)

- 태그(tag): `<p></p>`와 같은 태그 자체
- 요소(element): `<p>hello world</p>`에서 `<p>`태그 및 내부 부분을 모두 포함

> `<p>`태그를 적용한 스타일이라는 표현은 옳지 않음. p요소에 적용하는 스타일로 표현하는 것이 옳바름.

---

## 2. 전체 셀렉터(Universal Selector)

전체 셀렉터는 스타일을 문서의 모든 요소에 적용할 때 사용. html 요소를 포함한 요소가 선택됨.(head 요소도 포함)

기본형 `* {속성: 값;}`

```html
...
<head>
  <style>
    /* 모든 요소를 선택 */
    * {
      color: red;
    }
  </style>
</head>
<body>
  <h1>Welcome my TIL</h1>
  <p>반갑습니다.</p>
  <span>안녕하세요.</span>
</body>
```

---

## 3. 태그/타입 셀렉터(Type Selector)

태그 셀렉터는 특정 태그를 사용한 모든 요소에 스타일을 적용. 태그 선택자를 사용해 스타일을 지정하면 해당 태그를 사용한 모든 요소에 적용.

기본형 ` 태그명 { 스타일 규칙 }`

```html
<head>
  <style>
    /* 모든 p 태그 요소를 선택 */
    p {
      color: red;
    }
  </style>
</head>
<body>
  <h1>Welcome my TIL</h1>
  <p>반갑습니다.</p>
  <p>아래의 내용을 보세요.</p>
  <span>안녕하세요.</span>
</body>
```

---

## 4. ID 셀렉터(ID Selector)

id 어트리뷰트 값을 지정하여 일치하는 요소를 선택. id 어트리뷰트 값은 중복될 수 없는 유일한 값이므로 주로 문서의 레이아웃과 관련된 스타일을 지정하는데 사용.

기본형 `#id 어트리뷰트 값 { 스타일 규칙 }`

```html
<head>
  <style>
    /* id 어트리뷰트 값이 header인 요소를 선택 */
    #header {
      color: red;
    }
  </style>
</head>
<body>
  <header id="header">
    <h1>Welcome my TIL</h1>
    <p>반갑습니다.</p>
    <p>아래의 내용을 보세요.</p>
    <span>안녕하세요.</span>
  </header>
</body>
```

---

## 5. 클래스 셀렉터(Class Selector)

class 어트리뷰트 값을 지정하여 일치하는 요소를 선택. class 어트리뷰트 값은 중복될 수 있음.

기본형 `.class 어트리뷰트 값 { 스타일 규칙 }`

```html
<head>
  <style>
    /* 모든 p 태그 요소를 선택 */
    p {
      background-color: black;
    }
    /* class 어트리뷰트 값이 text1인 모든 요소를 선택 */
    .text1 {
      color: yellow;
    }
    /* class 어트리뷰트 값이 text2인 모든 요소를 선택 */
    .text2 {
      color: #fff;
    }
    /* class 어트리뷰트 값이 text-large인 모든 요소를 선택 */
    .text-large {
      font-size: 200%;
    }
    /* class 어트리뷰트 값이 text-center인 모든 요소를 선택 */
    .text-center {
      text-align: center;
    }
  </style>
</head>
<body>
  <header id="header">
    <h1>Welcome my TIL</h1>
    <p class="text1 text-large">반갑습니다.</p>
    <p class="text1 text-center">아래의 내용을 보세요.</p>
    <p class="text2 text-large text-center">아래의 내용을 보세요.</p>
    <span>안녕하세요.</span>
  </header>
</body>
```

> HTML요소에 class 어트리뷰트 값은 공백으로 구분하여 여러 개 지정할 수 있음. 위와 같이 클래스 셀렉터를 사용하여 미리 스타일을 정의해 두고, HTML요소는 이미 정의되어 있는 클래스를 지정하는 것으로 필요한 스타일을 지정할 수 있음. 이는 **재사용**의 측면에서 유용함.
>
> > Tailwind Css의 개념이지 않을까? 싶다.  
> > [공식문서](https://tailwindcss.com/)

---
