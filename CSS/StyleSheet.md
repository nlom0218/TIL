# Style Sheet

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. Inline Style](#1-inline-style์ธ๋ผ์ธ-์คํ์ผ)
- [2. Internal Style Sheet](#2-internal-style-sheet๋ด๋ถ-์คํ์ผ-์ํธ-embedding-style)
- [3. Linking Style Sheet](#3-linking-style-sheet์ธ๋ถ-์คํ์ผ-์ํธ-link-style)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. Inline Style(์ธ๋ผ์ธ ์คํ์ผ)

๊ฐ๋จํ ์คํ์ผ ์ ๋ณด๋ฅผ ์คํ์ผ ์ํธ๋ฅผ ์ฌ์ฉํ์ง ์๊ณ  ์คํ์ผ์ ์ ์ฉํ  ๋์์ ์ง์  ํ์ํ๋ ๋ฐฉ๋ฒ  
style="์์ฑ: ์์ฑ๊ฐ;"ํํ๋ก ์คํ์ผ์ ๋ฐ๊ฟ

```html
<p style="color: blue">Lorem ipsum dolor.</p>
```

- ๋จ์ 
  - ๊พธ๋ฏธ๋ ๋ฐ ํ๊ณ๊ฐ ์์
  - ์ฌ์ฌ์ฉ์ด ๋ถ๊ฐ๋ฅ

---

## 2. Internal Style Sheet(๋ด๋ถ ์คํ์ผ ์ํธ), Embedding style

์น ๋ฌธ์ ์์์ ์ฌ์ฉํ  ์คํ์ผ์ ๊ฐ์ ๋ฌธ์ ์์ ์ ๋ฆฌํ ๊ฒ  
`<head>`ํ๊ทธ ์์์ ์ ์ํ๊ณ  `<style>๊ณผ <style>`ํ๊ทธ ์ฌ์ด์ ์์ฑ

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

- ์ฅ์ 

  - HTML ๋ฌธ์ ์ ์ฌ๋ฌ ์์(ํ๊ทธ)๋ฅผ ํ๋ฒ์ ๊พธ๋ฐ ์ ์์

- ๋จ์ 
  - ๋ ๋ค๋ฅธ HTML ๋ฌธ์ธ์ด๋ ์ ์ฉํ  ์ ์์

---

## 3. Linking Style Sheet(์ธ๋ถ ์คํ์ผ ์ํธ), Link style

์ฌ๋ฌ ์น ๋ฌธ์์์ ์ฌ์ฉํ  ์คํ์ผ์ ๋ณ๋ ํ์ผ๋ก ์ ์ฅํด ๋๊ณ  ํ์ํ  ๋ ๋ง๋ค ๊ฐ์ ธ์์ ์ฌ์ฉ  
`*.css`๋ผ๋ ํ์ผ ํ์ฅ์๋ฅผ ์ฌ์ฉ

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

- ์ฅ์ 
  - ์ฌ๋ฌ HTML๋ฌธ์์ ์ฌ์ฉํ  ์ ์์

---

## ์ฐธ๊ณ 

[poiemaweb 2-1 CSS ๊ธฐ๋ณธ๋ฌธ๋ฒ](https://poiemaweb.com/css3-syntax)  
[CSS๋ฅผ HTML์ ์ ์ฉ์ํค๋ ๋ฐฉ๋ฒ](https://www.codingfactory.net/10529)  
๋์ - HTML + CSS + ์๋ฐ์คํฌ๋ฆฝํธ ์น ํ์ค์ ์ ์

---

[๐](#style-sheet)
