# Seletor

## π λ°λ‘κ°κΈ°

- [1. κ°μ](#1-κ°μ)
- [2. μ μ²΄ μλ­ν°(Universal Selector)](#2-μ μ²΄-μλ ν°universal-selector)
- [3. νκ·Έ/νμ μλ ν°(Type Selector)](#3-νκ·Ένμ-μλ ν°type-selector)
- [4. ID μλ ν°(ID Selector)](#4-id-μλ ν°id-selector)
- [5. ν΄λμ€ μλ ν°(Class Selector)](#5-ν΄λμ€-μλ ν°class-selector)
- [6. μ΄νΈλ¦¬λ·°νΈ μλ ν°(Attribute Selector)](#6-μ΄νΈλ¦¬λ·°νΈ-μλ ν°attribute-selector)
- [7. λ³΅ν© μλ ν° (Combinator)](#7-λ³΅ν©-μλ ν°-combinator)
  - [7-1. νμ μλ ν° (Descendant Combinator)](#7-1-νμ-μλ ν°-descendant-combinator)
  - [7-2. μμ μλ ν° (Child Combinator)](#7-2-μμ-μλ ν°-child-combinator)
  - [7-3. νμ (λμ) μλ ν° (Sibling Combinator)](#7-3-νμ λμ-μλ ν°-sibling-combinator)
    - [7-3-1. μΈμ  νμ  μλ ν° (Adjacent Sibling Combinator)](#7-3-1-μΈμ -νμ -μλ ν°-adjacent-sibling-combinator)
    - [7-3-2. μΌλ° νμ  μλ ν° (General Sibling Combinator)](#7-3-2-μΌλ°-νμ -μλ ν°-general-sibling-combinator)
- [8. κ°μ ν΄λμ€ μλ ν° (Pseudo-Class Selector)](#8-κ°μ-ν΄λμ€-μλ ν°-pseudo-class-selector)
  - [8-1. λ§ν¬ μλ ν° (Link pseudo-classes), λμ  μλ ν° (User action pseudo-classes)](#8-1-λ§ν¬-μλ ν°-link-pseudo-classes-λμ -μλ ν°-user-action-pseudo-classes)
  - [8-2. UI μμ μν μλ ν° (UI element states pseudo-classes)](#8-2-ui-μμ-μν-μλ ν°-ui-element-states-pseudo-classes)
  - [8-3. κ΅¬μ‘° κ°μ ν΄λμ€ μλ ν° (Structural pseudo-classes)](#8-3-κ΅¬μ‘°-κ°μ-ν΄λμ€-μλ ν°-structural-pseudo-classes)
  - [8-4. λΆμ  μλ ν° (Negation pseudo-class)](#8-4-λΆμ -μλ ν°-negation-pseudo-class)
  - [8-5. μ ν©μ± μ²΄ν¬ μλ ν° (validity pseudo-class)](#8-5-μ ν©μ±-μ²΄ν¬-μλ ν°-validity-pseudo-class)
- [9. κ°μ μμ μλ ν° (Pseudo-Element Selector)](#9-κ°μ-μμ-μλ ν°-pseudo-element-selector)
- [μ°Έκ³ ](#μ°Έκ³ )

---

## 1. κ°μ

styleλ₯Ό μ μ© νκ³ μνλ HTMLμμλ₯Ό νΉμ ν  νμκ° μλ€. μ΄λ μ¬μ©νλ κ²μ΄ μλ ν°/μ νμ(Selector)μ΄λ€. styleλ₯Ό μ μ©νκ³ μνλ HTML μμλ₯Ό μλ ν°λ‘ νΉμ νκ³  μ νλ μμμ μ€νμΌμ μ μνλ κ². λ³΅μκ°μ μλ ν°λ₯Ό μ°μνμ¬ μ§μ ν  μ μμΌλ©° μΌν(,)λ‘ κ΅¬λΆν¨.
![CSS Rule Set](../image/CSS/CSSRuleSet.png)

- νκ·Έ(tag): `<p></p>`μ κ°μ νκ·Έ μμ²΄
- μμ(element): `<p>hello world</p>`μμ `<p>`νκ·Έ λ° λ΄λΆ λΆλΆμ λͺ¨λ ν¬ν¨

> `<p>`νκ·Έλ₯Ό μ μ©ν μ€νμΌμ΄λΌλ ννμ μ³μ§ μμ. pμμμ μ μ©νλ μ€νμΌλ‘ νννλ κ²μ΄ μ³λ°λ¦.

---

## 2. μ μ²΄ μλ ν°(Universal Selector)

μ μ²΄ μλ ν°λ μ€νμΌμ λ¬Έμμ λͺ¨λ  μμμ μ μ©ν  λ μ¬μ©. html μμλ₯Ό ν¬ν¨ν μμκ° μ νλ¨.(head μμλ ν¬ν¨)

κΈ°λ³Έν `* {μμ±: κ°;}`

```html
...
<head>
  <style>
    /* λͺ¨λ  μμλ₯Ό μ ν */
    * {
      color: red;
    }
  </style>
</head>
<body>
  <h1>Welcome my TIL</h1>
  <p>λ°κ°μ΅λλ€.</p>
  <span>μλνμΈμ.</span>
</body>
```

---

## 3. νκ·Έ/νμ μλ ν°(Type Selector)

νκ·Έ μλ ν°λ νΉμ  νκ·Έλ₯Ό μ¬μ©ν λͺ¨λ  μμμ μ€νμΌμ μ μ©. νκ·Έ μ νμλ₯Ό μ¬μ©ν΄ μ€νμΌμ μ§μ νλ©΄ ν΄λΉ νκ·Έλ₯Ό μ¬μ©ν λͺ¨λ  μμμ μ μ©.

κΈ°λ³Έν ` νκ·Έλͺ { μ€νμΌ κ·μΉ }`

```html
<head>
  <style>
    /* λͺ¨λ  p νκ·Έ μμλ₯Ό μ ν */
    p {
      color: red;
    }
  </style>
</head>
<body>
  <h1>Welcome my TIL</h1>
  <p>λ°κ°μ΅λλ€.</p>
  <p>μλμ λ΄μ©μ λ³΄μΈμ.</p>
  <span>μλνμΈμ.</span>
</body>
```

---

## 4. ID μλ ν°(ID Selector)

id μ΄νΈλ¦¬λ·°νΈ κ°μ μ§μ νμ¬ μΌμΉνλ μμλ₯Ό μ ν. id μ΄νΈλ¦¬λ·°νΈ κ°μ μ€λ³΅λ  μ μλ μ μΌν κ°μ΄λ―λ‘ μ£Όλ‘ λ¬Έμμ λ μ΄μμκ³Ό κ΄λ ¨λ μ€νμΌμ μ§μ νλλ° μ¬μ©.

κΈ°λ³Έν `#id μ΄νΈλ¦¬λ·°νΈ κ° { μ€νμΌ κ·μΉ }`

```html
<head>
  <style>
    /* id μ΄νΈλ¦¬λ·°νΈ κ°μ΄ headerμΈ μμλ₯Ό μ ν */
    #header {
      color: red;
    }
  </style>
</head>
<body>
  <header id="header">
    <h1>Welcome my TIL</h1>
    <p>λ°κ°μ΅λλ€.</p>
    <p>μλμ λ΄μ©μ λ³΄μΈμ.</p>
    <span>μλνμΈμ.</span>
  </header>
</body>
```

---

## 5. ν΄λμ€ μλ ν°(Class Selector)

class μ΄νΈλ¦¬λ·°νΈ κ°μ μ§μ νμ¬ μΌμΉνλ μμλ₯Ό μ ν. class μ΄νΈλ¦¬λ·°νΈ κ°μ μ€λ³΅λ  μ μμ.

κΈ°λ³Έν `.class μ΄νΈλ¦¬λ·°νΈ κ° { μ€νμΌ κ·μΉ }`

```html
<head>
  <style>
    /* λͺ¨λ  p νκ·Έ μμλ₯Ό μ ν */
    p {
      background-color: black;
    }

    /* class μ΄νΈλ¦¬λ·°νΈ κ°μ΄ text1μΈ λͺ¨λ  μμλ₯Ό μ ν */
    .text1 {
      color: yellow;
    }

    /* class μ΄νΈλ¦¬λ·°νΈ κ°μ΄ text2μΈ λͺ¨λ  μμλ₯Ό μ ν */
    .text2 {
      color: #fff;
    }

    /* class μ΄νΈλ¦¬λ·°νΈ κ°μ΄ text-largeμΈ λͺ¨λ  μμλ₯Ό μ ν */
    .text-large {
      font-size: 200%;
    }

    /* class μ΄νΈλ¦¬λ·°νΈ κ°μ΄ text-centerμΈ λͺ¨λ  μμλ₯Ό μ ν */
    .text-center {
      text-align: center;
    }
  </style>
</head>
<body>
  <header id="header">
    <h1>Welcome my TIL</h1>
    <p class="text1 text-large">λ°κ°μ΅λλ€.</p>
    <p class="text1 text-center">μλμ λ΄μ©μ λ³΄μΈμ.</p>
    <p class="text2 text-large text-center">μλμ λ΄μ©μ λ³΄μΈμ.</p>
    <span>μλνμΈμ.</span>
  </header>
</body>
```

> HTMLμμμ class μ΄νΈλ¦¬λ·°νΈ κ°μ κ³΅λ°±μΌλ‘ κ΅¬λΆνμ¬ μ¬λ¬ κ° μ§μ ν  μ μμ. μμ κ°μ΄ ν΄λμ€ μλ ν°λ₯Ό μ¬μ©νμ¬ λ―Έλ¦¬ μ€νμΌμ μ μν΄ λκ³ , HTMLμμλ μ΄λ―Έ μ μλμ΄ μλ ν΄λμ€λ₯Ό μ§μ νλ κ²μΌλ‘ νμν μ€νμΌμ μ§μ ν  μ μμ. μ΄λ **μ¬μ¬μ©**μ μΈ‘λ©΄μμ μ μ©ν¨.
>
> > Tailwind Cssμ κ°λμ΄μ§ μμκΉ? μΆλ€.  
> > [κ³΅μλ¬Έμ](https://tailwindcss.com/)

---

## 6. μ΄νΈλ¦¬λ·°νΈ μλ ν°(Attribute Selector)

- μ§μ λ μ΄νΈλ¦¬λ·°νΈλ₯Ό κ°λ λͺ¨λ  μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ] { μ€νμΌ κ·μΉ }`
- μ§μ λ μ΄νΈλ¦¬λ·°νΈλ₯Ό κ°μ§λ©° μ§μ λ κ°κ³Ό μ΄νΈλ¦¬λ·°νΈμ κ°μ΄ μΌμΉνλ λͺ¨λ  μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ="κ°"] { μ€νμΌ κ·μΉ }`
- μ§μ λ μ΄νΈλ¦¬λ·°νΈμ κ°μ΄ (κ³΅λ°±μΌλ‘ λΆλ¦¬λ) λ¨μ΄λ‘ ν¬ν¨νλ μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ~="κ°"] { μ€νμΌ κ·μΉ }`
- μ§μ λ μ΄νΈλ¦¬λ·°νΈμ κ°κ³Ό μΌμΉνκ±°λ μ§μ  μ΄νΈλ¦¬λ·°νΈ κ° λ€ μ°μ΄μ νμ΄ν°("κ°-")μΌλ‘ μμνλ μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ|="κ°"] { μ€νμΌ κ·μΉ }`
- μ§μ λ μ΄νΈλ¦¬λ·°νΈ κ°μΌλ‘ μμνλ μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ^="κ°"] { μ€νμΌ κ·μΉ }`
- μ§μ λ μ΄νΈλ¦¬λ·°νΈ κ°μΌλ‘ λλλ μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ$="κ°"] { μ€νμΌ κ·μΉ }`
- μ§μ λ μ΄νΈλ¦¬λ·°νΈ κ°μ ν¬ν¨νλ μμλ₯Ό μ ν
  - κΈ°λ³Έν `μλ ν°[μ΄νΈλ¦¬λ·°νΈ*="κ°"] { μ€νμΌ κ·μΉ }`

```html
<head>
  <style>
    /* a μμ μ€μ href μ΄νΈλ¦¬λ·°νΈλ₯Ό κ°λ λͺ¨λ  μμ */
    a[href] {
      color: red;
    }

    /* a μμ μ€μ target μ΄νΈλ¦¬λ·°νΈ κ°μ΄ "_blank"μΈ λͺ¨λ  μμ */
    a[target="_blank"] {
      font-size: 28px;
    }

    /* h1 μμ μ€μ title μ΄νΈλ¦¬λ·°νΈ κ°μ "first"λ₯Ό λ¨μ΄λ₯Ό ν¬ν¨νλ μμ */
    h1[title~="first"] {
      color: blue;
    }

    /* h1 μμ μ€μ lang μ΄νΈλ¦¬λ·°νΈ κ°μ΄ "en"κ³Ό μΌμΉνκ±°λ "en-"λ‘ μμνλ μμ */
    h1[lang|="en"] {
      background-color: yellow;
    }

    /* a μμ μ€μ href μ΄νΈλ¦¬λ·°νΈ κ°μ΄ "https://"λ‘ μμνλ μμ  */
    a[href^="https://"]
    {
      font-weight: 700;
    }

    /* a μμ μ€μ href μ΄νΈλ¦¬λ·°νΈ κ°μ΄ ".html"λ‘ λλλ μμ */
    a[href$=".html"] {
      color: green;
    }

    /* h1 μμ μ€μμ class μ΄νΈλ¦¬λ·°νΈ κ°μ "test"λ₯Ό ν¬ν¨νλ μμ */
    h1[class*="test"] {
      font-style: italic;
    }
  </style>
</head>
<body>
  <header id="header">
    <a href="https://www.poiemaweb.com">poiemaweb.com</a><br />
    <a href="https://www.google.com" target="_blank">google.com</a><br />
    <a href="https://www.naver.com" target="_top">naver.com</a><br />
    <a href="http://www.test.com" target="_top">test.com</a>
    <h1 title="heading first" lang="en" class="first_test">Heading first</h1>
    <h1 title="heading-first" lang="en-us" class="second">Heading-first</h1>
    <h1 title="heading second" lang="en-gb" class="test">Heading second</h1>
    <h1 title="heading third" lang="us" class="second_test">Heading third</h1>
    <h1 title="heading third" lang="no">Heading third</h1>
    <a href="test.html">test.html</a><br />
    <a href="test.jsp">test.jsp</a>
  </header>
</body>
```

![AttributeSelector](../image/CSS/AttributeSelector.png)

---

## 7. λ³΅ν© μλ ν° (Combinator)

### 7-1. νμ μλ ν° (Descendant Combinator)

> μμ μ 1 level μμμ μνλ μμλ₯Ό λΆλͺ¨ μμ, 1 level νμμ μνλ μμλ₯Ό μμ μμ(μμ μμ)λΌ ν¨.
> μμ λ³΄λ€ n level νμμ μνλ μνλ μμλ νμ μμ(νμ μμ)λΌ ν¨.

```html
<body>
  <div>
    <h2>λλ μμμ΄λ©΄μ νμμ΄μμ.</h2>
    <p>λλ μμμ΄λ©΄μ νμμ΄μμ.</p>
    <span>
      <p>λλ νμμ΄μμ.</p>
    </span>
  </div>
</body>
```

κΈ°λ³Έν `μλ ν°A μλ ν°B { μ€νμΌ κ·μΉ }`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* div μμμ νμμμ μ€ p μμ */
      div p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Heading</h1>
    <div>
      <p>paragraph 1</p>
      <p>paragraph 2</p>
      <span><p>paragraph 3</p></span>
    </div>
    <p>paragraph 4</p>
  </body>
</html>
```

paragraph 4λ₯Ό μ μΈν λλ¨Έμ§ κ²λ€μ μ€νμΌμ΄ μ μ©λ¨.

---

### 7-2. μμ μλ ν° (Child Combinator)

μμ μλ ν°λ μλ ν°Aμ λͺ¨λ  μμ μμ μ€ μλ ν°Bμ μΌμΉνλ μμλ₯Ό μ ν. νμμ μλ.

κΈ°λ³Έν `μλ ν°A > μλ ν°B { μ€νμΌ κ·μΉ }`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* div μμμ μμμμ μ€ p μμ */
      div > p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Heading</h1>
    <div>
      <p>paragraph 1</p>
      <p>paragraph 2</p>
      <span><p>paragraph 3</p></span>
    </div>
    <p>paragraph 4</p>
  </body>
</html>
```

paragraph 1, 2λ§ μ€νμΌμ΄ μ μ©λ¨.

---

### 7-3. νμ (λμ) μλ ν° (Sibling Combinator)

νμ (λμ) μλ ν°λ νμ  κ΄κ³(λμ κ΄κ³)μμ λ€μ μμΉνλ μμλ₯Ό μ νν  λ μ¬μ©νλ€.

![Sibling Combinator](../image/CSS/Sibling%20Combinator.png)

---

#### 7-3-1. μΈμ  νμ  μλ ν° (Adjacent Sibling Combinator)

μλ ν°Aμ νμ  μμ μ€ μλ ν°A **λ°λ‘ λ€**μ μμΉνλ μλ ν°B μμλ₯Ό μ ν. Aμ Bμ¬μ΄μ λ€λ₯Έ μμκ° μ‘΄μ¬νλ©΄ μ νλμ§ μμ.

κΈ°λ³Έν `μλ ν°A + μλ ν°B { μ€νμΌ κ·μΉ }`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* p μμμ νμ  μμ μ€μ p μμ λ°λ‘ λ€μ μμΉνλ ul μμλ₯Ό μ ννλ€. */
      p + ul {
        color: red;
      }
    </style>
  </head>
  <body>
    <div>A div element.</div>
    <ul>
      <li>Coffee</li>
      <li>Tea</li>
      <li>Milk</li>
    </ul>

    <p>The first paragraph.</p>
    <!-- μλμ ulμμ(νμμμ ν¬ν¨)κ° μ€νμΌμ΄ μ μ©λλ€. -->
    <ul>
      <li>Coffee</li>
      <li>Tea</li>
      <li>Milk</li>
    </ul>

    <h2>Another list</h2>
    <ul>
      <li>Coffee</li>
      <li>Tea</li>
      <li>Milk</li>
    </ul>
  </body>
</html>
```

#### 7-3-2. μΌλ° νμ  μλ ν° (General Sibling Combinator)

μλ ν°Aμ νμ  μμ μ€ μλ ν°A λ€μ μμΉνλ μλ ν°B μμλ₯Ό **λͺ¨λ** μ ν

κΈ°λ³Έν `μλ ν°A ~ μλ ν°B { μ€νμΌ κ·μΉ }`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* p μμμ νμ  μμ μ€μ p μμ λ€μ μμΉνλ ul μμλ₯Ό λͺ¨λ μ ννλ€. */
      p ~ ul {
        color: red;
      }
    </style>
  </head>
  <body>
    <div>A div element.</div>
    <ul>
      <li>Coffee</li>
      <li>Tea</li>
      <li>Milk</li>
    </ul>

    <p>The first paragraph.</p>
    <!-- μλμ ulμμ(νμμμ ν¬ν¨)κ° μ€νμΌμ΄ μ μ©λλ€. -->
    <ul>
      <li>Coffee</li>
      <li>Tea</li>
      <li>Milk</li>
    </ul>

    <h2>Another list</h2>
    <!-- μλμ ulμμ(νμμμ ν¬ν¨)κ° μ€νμΌμ΄ μ μ©λλ€. -->
    <ul>
      <li>Coffee</li>
      <li>Tea</li>
      <li>Milk</li>
    </ul>
  </body>
</html>
```

---

## 8. κ°μ ν΄λμ€ μλ ν° (Pseudo-Class Selector)

κ°μ ν΄λμ€λ μμμ νΉμ  μνμ λ°λΌ μ€νμΌμ μ μν  λ μ¬μ©.

> - λ§μ°μ€κ° μ¬λΌμ μμ λ
> - λ§ν¬λ₯Ό λ°©λ¬Ένμ λμ μμ§ λ°©λ¬Ένμ§ μμμ λ
> - ν¬μ»€μ€κ° λ€μ΄μ μμ λ

μ΄λ¬ν νΉμ  μνμλ μλ ν΄λμ€κ° μ‘΄μ¬νμ§ μμ§λ§ κ°μ ν΄λμ€λ₯Ό μμλ‘ μ§μ νμ¬ μ ννλ λ°©λ²  
κ°μ ν΄λμ€λ λ§μΉ¨ν(.) λμ  μ½λ‘ (:)μ μ¬μ©, κ°μ ν΄λμ€λ λ―Έλ¦¬ μ μλ μ΄λ¦μ΄ μκΈ° λλ¬Έμ μμμ μ΄λ¦μ μ¬μ©ν  μ μμ.

μ¬μ©λ² `μλ ν°:κ°μν΄λμ€ { ν΄λμ€ κ·μΉ }`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* a μμκ° hover μνμΌ λ */
      a:hover {
        color: red;
      }
      /* input μμκ° focus μνμΌ λ */
      input:focus {
        background-color: yellow;
      }
    </style>
  </head>
  <body>
    <a href="#">Hover me</a><br /><br />
    <input type="text" placeholder="focus me" />
  </body>
</html>
```

---

### 8-1. λ§ν¬ μλ ν° (Link pseudo-classes), λμ  μλ ν° (User action pseudo-classes)

| pseudo-class | Description                      |
| :----------- | :------------------------------- |
| :link        | μλ ν°κ° λ°©λ¬Ένμ§ μλ λ§ν¬μΌ λ |
| :visited     | μλ ν°κ° λ°©λ¬Έν λ§ν¬μΌ λ        |
| :hover       | μλ ν°κ° λ§μ°μ€κ° μ¬λΌμ μμ λ |
| :active      | μλ ν°κ° ν΄λ¦­λ μνμΌ λ        |
| :focus       | μλ ν°μ ν¬μ»€μ€κ° λ€μ΄μ μμ λ |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* a μμκ° λ°©λ¬Ένμ§ μμ λ§ν¬μΌ λ */
      a:link {
        color: red;
      }

      /* a μμκ° λ°©λ¬Έν λ§ν¬μΌ λ */
      a:visited {
        color: blue;
      }

      /* a μμμ λ§μ°μ€κ° μ¬λΌμ μμ λ */
      a:hover {
        font-weight: 700;
      }

      /* a μμκ° ν΄λ¦­λ μνμΌ λ */
      a:active {
        color: green;
      }

      /* text input μμμ password input μμμ ν¬μ»€μ€κ° λ€μ΄μ μμ λ */
      input[type="text"]:focus,
      input[type="password"]:focus {
        color: red;
      }
    </style>
  </head>
  <body>
    <a href="#" target="_blank">This is a link</a><br />
    <input type="text" value="I'll be red when focused" /><br />
    <input type="password" value="I'll be red when focused" />
  </body>
</html>
```

---

### 8-2. UI μμ μν μλ ν° (UI element states pseudo-classes)

| pseudo-class | Description                      |
| :----------- | :------------------------------- |
| :checked     | μλ ν°κ° μ²΄ν¬ μνμΌ λ          |
| :enabled     | μλ ν°κ° μ¬μ© κ°λ₯ν μνμΌ λ   |
| :disabled    | μλ ν°κ° μ¬μ© λΆκ°λ₯ν μνμΌ λ |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* input μμκ° μ¬μ© κ°λ₯ν μνμΌ λ, input μμ λ°λ‘ λ€μ μμΉνλ μΈμ  νμ  span μμλ₯Ό μ ν */
      input:enabled + span {
        color: red;
      }

      /* input μμκ° μ¬μ© λΆκ°λ₯ν μνμΌ λ, input μμ λ°λ‘ λ€μ μμΉνλ μΈμ  νμ  span μμλ₯Ό μ ν */
      input:disabled + span {
        color: blue;
      }

      /* input μμκ° μ²΄ν¬ μνμΌ λ, input μμ λ°λ‘ λ€μ μμΉνλ μΈμ  νμ  span μμλ₯Ό μ ν */
      input:checked + span {
        color: red;
      }
    </style>
  </head>
  <body>
    <input type="radio" checked="checked" value="male" name="gender" />
    <span>Male</span><br />
    <input type="radio" value="female" name="gender" /> <span>Female</span
    ><br />
    <input type="radio" value="neuter" name="gender" disabled />
    <span>Neuter</span>
    <hr />

    <input type="checkbox" checked="checked" value="bicycle" />
    <span>I have a bicycle</span><br />
    <input type="checkbox" value="car" /> <span>I have a car</span><br />
    <input type="checkbox" value="motorcycle" disabled />
    <span>I have a motorcycle</span>
  </body>
</html>
```

---

### 8-3. κ΅¬μ‘° κ°μ ν΄λμ€ μλ ν° (Structural pseudo-classes)

| pseudo-class | Description                                                                                    |
| :----------- | :--------------------------------------------------------------------------------------------- |
| :first-child | μλ ν°μ ν΄λΉνλ λͺ¨λ  μμ μ€ μ²«λ²μ§Έ μμμΈ μμλ₯Ό μ ν, νμ  μμ μ€ μ μΌ μ²« μμλ₯Ό μ ν     |
| :last-child  | μλ ν°μ ν΄λΉνλ λͺ¨λ  μμ μ€ λ§μ§λ§ μμμΈ μμλ₯Ό μ ν, νμ  μμ μ€ μ μΌ λ§μ§λ§ μμλ₯Ό μ ν |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* p μμ μ€μμ μ²«λ²μ§Έ μμμ μ ν */
      p:first-child {
        color: red;
      }

      /* p μμ μ€μμ λ§μ§λ§ μμμ μ ν */
      /* body μμμ λλ²μ§Έ p μμλ λ§μ§λ§ μμ μμκ° μλλ€.
       body μμμ λ§μ§λ§ μμ μμλ div μμμ΄λ€. */
      p:last-child {
        color: blue;
      }
    </style>
  </head>
  <body>
    <p>This paragraph is the first child of its parent (body).</p>

    <h1>Welcome to My Homepage</h1>
    <p>This text isn't selected: it's not last child.</p>

    <div>
      <p>This paragraph is the first child of its parent (div).</p>
      <p>This paragraph is not the first child of its parent.</p>
    </div>
  </body>
</html>
```

![Structural pseudo-classes](../image/CSS/StructuralPseudoClasses1.png)

| pseudo-class       | Description                                                                         |
| :----------------- | :---------------------------------------------------------------------------------- |
| :nth-child(n)      | μλ ν°μ ν΄λΉνλ λͺ¨λ  μμ μ€ μμμ nλ²μ§Έ μμμΈ μμλ₯Ό μ ν                      |
| :nth-last-child(n) | μλ ν°μ ν΄λΉνλ λͺ¨λ  μμ μ€ λ€μμ nλ²μ§Έ μμμΈ μμλ₯Ό μ ν                      |
| :first-of-type     | μλ ν°μ ν΄λΉνλ μμμ λΆλͺ¨ μμμ μμ μμ μ€ μ²«λ²μ§Έ λ±μ₯νλ μμλ₯Ό μ ν       |
| :last-of-type      | μλ ν°μ ν΄λΉνλ μμμ λΆλͺ¨ μμμ μμ μμ μ€ λ§μ§λ§μ λ±μ₯νλ μμλ₯Ό μ ν     |
| :nth-of-type(n)    | μλ ν°μ ν΄λΉνλ μμμ λΆλͺ¨ μμμ μμ μμ μ€ μμμ nλ²μ§Έ λ±μ₯νλ μμλ₯Ό μ ν |
| :nth-last-type(n)  | μλ ν°μ ν΄λΉνλ μμμ λΆλͺ¨ μμμ μμ μμ μ€ λ€μμ nλ²μ§Έ λ±μ₯νλ μμλ₯Ό μ ν |

> κ΅¬μ‘° κ°μ μλ ν°λ μκ°λ³΄λ€ κ΅¬λ³νκΈ°κ° μ΄λ ΅λ€. μ¬λ¬λ² μ°λ©΄μ μ΅μν΄μ§λλ‘ λΈλ ₯νκΈ°  
> κ°κ°μ κ΅¬μ‘° κ°μ μλ ν°λ§λ€ νΉμ§μ΄ μλ€. λͺ¨λ λ€λ₯΄λ―λ‘ μν©μ λ§κ² κ³ λ―Όνλ©° μ¬μ©νκΈ°  
> μν©μ λ°λΌμλ κ°μ μμλ₯Ό μ ννλ κ²½μ°λ μλ€.

---

### 8-4. λΆμ  μλ ν° (Negation pseudo-class)

μλ ν°μ ν΄λΉνμ§ μλ λͺ¨λ  μμλ₯Ό μ ν

μ¬μ©λ² `:not(μλ ν°) { μ€νμΌ κ·μΉ }`

```html
<!DOCTYPE html>
<head>
  <style>
    /* input μμ μ€μμ type μ΄νΈλ¦¬λ·°νΈ κ°μ΄ passwordκ° μλ μμλ₯Ό μ ν */
    input:not([type="password"]) {
      color: red;
    }
  </style>
</head>
<body>
  <input type="text" value="Text input" />
  <input type="email" value="email input" />
  <input type="password" value="Password input" />
</body>
```

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      max-width: 300px;
      padding: 10px;
      margin-bottom: 10px;
    }
    /* div μμ μ€μμ 1, 4, 7...λ²μ§Έ λ±μ₯νλ μμκ° μλ μμλ§μ μ ν */
    /* 1, 4, 7... : κ³΅μ°¨κ° 3μΈ λ±μ°¨μμ΄ */
    div:not(:nth-child(3n + 1)) {
      color: red;
    }

    /* div μμ μ€μμ 4λ²μ§Έ μ΄ν λ±μ₯νλ μμκ° μλ μμλ§μ μ ν */
    div:not(:nth-of-type(n + 4)) {
      background-color: yellow;
    }
  </style>
</head>
<body>
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
  <div>6</div>
  <div>7</div>
</body>
```

> μμ κ²½μ° `nth-child()`κ³Ό `nth-of-type()`μ€ μλ¬΄κ±°λ μ¬μ©ν΄λ κ°μ μμλ€μ΄ μ νλλ€.

![NegationPseudoClass](../image/CSS/NegationPseudoClass.png)

---

### 8-5. μ ν©μ± μ²΄ν¬ μλ ν° (validity pseudo-class)

| pseudo-class | Description                                                |
| :----------- | :--------------------------------------------------------- |
| :valid       | μ ν©μ± κ²μ¦μ΄ μ±κ³΅ν input μμ λλ form μμλ₯Ό μ ννλ€. |
| :invalid     | μ ν©μ± κ²μ¦μ΄ μ€ν¨ν input μμ λλ form μμλ₯Ό μ ννλ€. |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* μ ν©μ± κ²μ¦μ μ±κ³΅ν κ²½μ° */
      input[type="text"]:valid {
        background-color: white;
      }

      /* μ ν©μ± κ²μ¦μ μ€ν¨ν κ²½μ° */
      input[type="text"]:invalid {
        background-color: red;
      }
    </style>
  </head>
  <body>
    <label
      >μλ ₯κ°μ΄ λ°λμ νμ
      <input type="text" required />
    </label>
  </body>
</html>
```

![ValidityPseudoClass](../image/CSS/ValidityPseudoClass.png)

---

## 9. κ°μ μμ μλ ν° (Pseudo-Element Selector)

κ°μ μμλ μμμ νΉμ  λΆλΆμ μ€νμΌμ μ μ©νκΈ° μνμ¬ μ¬μ©

- μμ μ½νμΈ μ μ²«κΈμ λλ μ²«μ€
- μμ μ½μ²ΈμΈ μ μ λλ λ€

μ¬μ©λ² `μλ ν°::pseudo-element { μ€νμΌ κ·μΉ}`

| pseudo-class   | Description                                                                       |
| :------------- | :-------------------------------------------------------------------------------- |
| ::first-letter | μ½νμΈ μ μ²«κΈμλ₯Ό μ ν                                                            |
| ::first-line   | μ½νμΈ μ μ²«μ€μ μ ν, **λΈλ‘** μμμλ§ μ μ©                                      |
| ::after        | μ½νμΈ μ λ€μ μμΉνλ κ³΅κ°μ μ ν, μΌλ°μ μΌλ‘ **`content`** νλ‘νΌν°μ ν¨κ» μ¬μ© |
| ::before       | μ½νμΈ μ μμ μμΉνλ κ³΅κ°μ μ ν, μΌλ°μ μΌλ‘ **`content`** νλ‘νΌν°μ ν¨κ» μ¬μ© |
| ::selection    | λλκ·Έ ν μ½νμΈ λ₯Ό μ ν, iOS Safari λ± μΌλΆ λΈλΌμ°μ μμλ λμνμ§ μμ          |

```html
<!DOCTYPE html>
<head>
  <style>
    /* p μμ μ½νμΈ μ μ²«κΈμλ₯Ό μ ν */
    p::first-letter {
      font-size: 40px;
    }

    /* p μμ μ½νμΈ μ μ²«μ€μ μ ν */
    p::first-line {
      color: blue;
    }

    /* h1 μμ μ½νμΈ μ μ κ³΅κ°μ content μ΄νΈλ¦¬λ·°νΈ κ°μ μ½μ */
    h1::after {
      content: "HELLO!! ";
      color: red;
    }

    /* h1 μμ μ½νμΈ μ λ£ κ³΅κ°μ content μ΄νΈλ¦¬λ·°νΈ κ°μ μ½μ */
    h1::before {
      content: " WORLD!!";
      color: green;
    }

    /* p μμ μ€ λλκ·Έν μ½νμΈ λ₯Ό μ ν */
    p::selection {
      background: yellow;
    }
  </style>
</head>
<body>
  <h1>μ λͺ©μλλ€.</h1>
  <p>
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. Explicabo illum
    sunt distinctio sed, tempore,<br />
    repellat rerum et ea laborum voluptatum! Quisquam error fugiat debitis
    maiores officiis, tenetur ullam amet in!
  </p>
</body>
```

## ![PseudoElementSelector](../image/CSS/PseudoElementSelector.png)

---

## μ°Έκ³ 

[poiemaweb 2-2 μλ ν°](https://poiemaweb.com/css3-selector)  
λμ - HTML + CSS + μλ°μ€ν¬λ¦½νΈ μΉ νμ€μ μ μ

---

[π](#seletor)
