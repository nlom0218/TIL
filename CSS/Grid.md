# Grid Layout

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. ์ฉ์ด ์ ๋ฆฌ](#2-์ฉ์ด-์ ๋ฆฌ)
- [3. Usage](#3-usage)
- [4. ๊ทธ๋ฆฌ๋ ํํ ์ ์](#4-๊ทธ๋ฆฌ๋-ํํ-์ ์)
  - [4-1. grid-template-columns](#4-1-grid-template-columns)
  - [4-2. grid-template-rows](#4-2-grid-template-rows)
  - [4-3. repeat ํจ์](#4-3-repeat-ํจ์)
  - [4-4. minmax ํจ์](#4-4-minmax-ํจ์)
  - [4-5. auto-fill, auto-fit](#4-5-auto-fill-auto-fit)
- [5. ๊ฐ๊ฒฉ๋ง๋ค๊ธฐ](#5-๊ฐ๊ฒฉ๋ง๋ค๊ธฐ)
- [6. ๊ทธ๋ฆฌ๋ ํํ๋ฅผ ์๋์ผ๋ก ์ ์](#6-๊ทธ๋ฆฌ๋-ํํ๋ฅผ-์๋์ผ๋ก-์ ์)
- [7. grid-column, grid-row](#7-grid-column-grid-row)
- [8. ์ธ๋ก ๋ฐฉํฅ ์ ๋ ฌ](#8-์ธ๋ก-๋ฐฉํฅ-์ ๋ ฌ)
  - [8-1. align-items](#8-1-align-items)
  - [8-2. align-content](#8-2-align-content)
  - [8-3. align-self](#8-3-align-self)
- [9. ๊ฐ๋ก ๋ฐฉํฅ ์ ๋ ฌ](#9-๊ฐ๋ก-๋ฐฉํฅ-์ ๋ ฌ)
  - [9-1. justify-items](#9-1-justify-items)
  - [9-2. justify-content](#9-2-justify-content)
  - [9-3. justify-self](#9-3-justify-self)
- [10. ์ ๋ ฌ shorthand](#10-์ ๋ ฌ-shorthand)
  - [10-1. place-content](#10-1-place-content)
  - [10-2. place-slef](#10-2-place-slef)
- [11. order](#11-order)
- [12. z-index](#12-z-index)
- [13. Conclusion](#13-conclusion)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

ํ๋ ์น ๋ ์ด์์์ ๋ด๋นํ๊ณ  ์๋ ๊ฐ์ฅ ํฐ ํ๋กํผํฐ๊ฐ ๋ฐ๋ก Grid์ด๋ค. ๋์ ๊ฒฝ์ฐ ์ ์ฒด์ ์ธ ํ์ Grid๋ก ์ก๊ณ  ์์ ๋ถ๋ถ ๋ถ๋ถ์ Flex๋ฅผ ์ฌ์ฉํ๊ณ  ์๋ค. ์ฌ์ค ์ง๊ธ์ ์์ ๋ถ๋ถ๋ Flex๋ณด๋ค๋ Grid๋ฅผ ์ฌ์ฉํ๋ ํธ์ด๋ค.

์ด์ ์ ์ฌ๋ฆฐ Flexbox Layout์ ํ ๋ฐฉํฅ ๋ ์ด์์ ์์คํ์ด๋ผ๋ฉด Grid Layout์ ๋ ๋ฐฉํฅ ๋ ์ด์์ ์์คํ์ด๋ค. ๋ฐ๋ผ์ Flex๋ณด๋ค ๋ ๋ณต์ก์ ์ธ ๋ ์ด์์ ํํ์ด ๊ฐ๋ฅํ๋ค.

---

## 2. ์ฉ์ด ์ ๋ฆฌ

![grid layout](../image/CSS/Grid/grid.png)

- ๊ทธ๋ฆฌ๋ ์ปจํ์ด๋(Grid Container): `display: grid`๋ฅผ ์ ์ฉํ๋, Grid์ ์ ์ฒด ์์ญ์ด๋ค. Grid ์ปจํ์ด๋ ์์ ์์๋ค์ด Grid ๊ท์น์ ์ํฅ์ ๋ฐ์ ์ ๋ ฌ๋๋ค.
- ๊ทธ๋ฆฌ๋ ์์ดํ(Grid Item): Grid ์ปจํ์ด๋์ ์์ ์์๋ค์ด๋ค. ๊ทธ๋ฆฌ๋ ์์ดํ๋ค์ด Grid ๊ท์น์ ์ํด ๋ฐฐ์น๋๋ค.
- ๊ทธ๋ฆฌ๋ ํธ๋(Grid Track): Grid์ ํ(Row) ๋๋ ์ด(Column)
- ๊ทธ๋ฆฌ๋ ์(Grid Cell): Grid์ ํ ์นธ์ ๊ฐ๋ฅดํจ๋ค. `<div>`๊ฐ์ ์ค์  html ์์๋ Grid ์์ดํ์ด๊ณ , ์ด๋ฐ Grid ์์ดํ ํ๋๊ฐ ๋ค์ด๊ฐ๋ '๊ฐ์์ ์นธ(ํ)'์ด Grid ์์ด๋ค.
- ๊ทธ๋ฆฌ๋ ๋ผ์ธ(Grid Line): Grid ์์ ๊ตฌ๋ถํ๋ ์ ์ด๋ค.
- ๊ทธ๋ฆฌ๋ ๋ฒํธ(Grid Number): Grid ๋ผ์ธ์ ๊ฐ ๋ฒํธ์ด๋ค.
- ๊ทธ๋ฆฌ๋ ๊ฐญ(Grid Gap): Grid ์ ์ฌ์ด์ ๊ฐ๊ฒฉ์ด๋ค.
- ๊ทธ๋ฆฌ๋ ์์ญ(Grid Area): Grid ๋ผ์ธ์ผ๋ก ๋๋ฌ์ธ์ธ ์ฌ๊ฐํ ์์ญ์ผ๋ก, ๊ทธ๋ฆฌ๋ ์์ ์งํฉ์ด๋ค.

---

## 3. Usage

Grid ๊ท์น์ ์ ์ฉํ๊ณ ์ ํ๋ ์์๋ค์ ๋ถ๋ชจ์ `display: grid;`๋ฅผ ์ ์ฉ์ ํ๋ฉด ๋๋ค.

```css
.grid-container {
  display: grid;
}
```

`display: inline-grid`๋ ์๋ค. ์์ดํ์ ๋ฐฐ์น์ ๊ด๋ จ์ด ์๋ค๊ฐ ๋ณด๋ค๋, ์ปจํ์ด๋๊ฐ ์ฃผ๋ณ ์์๋ค๊ณผ ์ด๋ป๊ฒ ์ด์ฐ๋ฌ์ง์ง ๊ฒฐ์ ํ๋ ๊ฐ์ด๋ค. `inline-grid`๋ `inline-block`์ฒ๋ผ ๋์ํ๋ค.

```css
.grid-container {
  display: inline-grid;
}
```

---

## 4. ๊ทธ๋ฆฌ๋ ํํ ์ ์

### 4-1. grid-template-columns

`grid-template-columns` ํ๋กํผํฐ๋ grid columns์ ๋ฐฐ์น๋ฅผ ์ง์ ํ๋๋ฐ ์ฌ์ฉํ๋ค.

```css
.grid-container {
  display: grid;
  grid-template-columns: 100px 200px 300px;
}
```

์์ ์ฝ๋๋ grid-container๋ก ๊ฐ์ธ์ ธ ์๋ ์์์์๋ค์ ๊ทธ๋ฆฌ๋๋ก ๋ฐฐ์นํ๊ณ  column์ 3์ด๋ก ๋ง๋ค๊ณ  ์ฒซ๋ฒ์งธ column ๋ถํฐ 100px, 200px, 300px๋งํผ์ ํฌ๊ธฐ๋ก ๋ง๋ค๊ฒ ๋ค๋ ์๋ฏธ์ด๋ค.

๋ ๋ค๋ฅธ ์ฌ์ฉ๋ฒ์ ๋ณด์.

```css
.grid-container {
  display: grid;
  grid-template-columns: 1fr 2fr 2fr;
}
```

์์ ์ฝ๋์์ fr์ fraction์ผ๋ก ์ซ์ ๋น์จ๋๋ก ๊ทธ๋ฆฌ๋ ํธ๋์ ํฌ๊ธฐ๋ฅผ ๋๋๋ค. ์ฆ `1fr 2fr 2fr`์ 1:2:2 ๋น์ค์ผ 3๊ฐ์ column์ ๋ง๋ค๊ฒ ๋ค๋ ์๋ฏธ์ด๋ค.

```css
.grid-container {
  display: grid;
  grid-template-columns: 100px 2fr 2fr;
}
```

์์ ์ฝ๋์ ๊ฐ์ด px๊ณผ fr๋ฅผ ํจ๊ป ์ฌ์ฉํ  ์ ์๋ค. %๋ก๋ ํฌ๊ธฐ๋ฅผ ์ง์ ํ  ์ ์๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container1 {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        margin-bottom: 20px;
      }
      .grid-container2 {
        display: grid;
        grid-template-columns: 160px 200px 300px;
        margin-bottom: 20px;
      }
      .grid-container3 {
        display: grid;
        grid-template-columns: 200px 1fr 400px;
      }
    </style>
  </head>
  <body>
    <div>
      grid-template-columns: 1fr 1fr 1fr;
      <div class="grid-container1">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      grid-template-columns: 160px 200px 300px;
      <div class="grid-container2">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      grid-template-columns: 200px 1fr 400px;
      <div class="grid-container3">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![grid-template-columns](../image/CSS/Grid/gridTemplateColumns.png)

---

### 4-2. grid-template-rows

`grid-template-columns`์ ์ด(column)์ ๋ฐฐ์น๋ผ๋ฉด `grid-template-rows`์ ํ(row)์ ๋ฐฐ์น์ด๋ค. ๊ทธ ์ธ์ ๋ด์ฉ์ ๊ฐ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container1 {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        grid-template-rows: 1fr 2fr;
        margin-bottom: 20px;
      }
      .grid-container2 {
        display: grid;
        grid-template-columns: 1fr 200px 300px;
        grid-template-rows: 160px 300px;
        margin-bottom: 20px;
      }
    </style>
  </head>
  <body>
    <div>
      grid-template-columns: 1fr 1fr 1fr; / grid-template-rows: 1fr 2fr;
      <div class="grid-container1">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      grid-template-columns: 1fr 200px 300px; / grid-template-rows: 160px 300px;
      <div class="grid-container2">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![grid-template-rows](../image/CSS/Grid/gridTemplateRows.png)

---

### 4-3. repeat ํจ์

`grid-template-columns, grid-template-rows`์ ํ๋กํผํฐ๊ฐ์ผ๋ก ์ฌ์ฉ๋๋ repeat๋ ๋ฐ๋ณต๋๋ ๊ฐ์ ์๋์ผ๋ก ์ฒ๋ฆฌํ  ์ ์๋ ํจ์์ด๋ค.

์ฌ์ฉ๋ฒ `grid-template-columns: repeat(๋ฐ๋ณตํ์, ๋ฐ๋ณต๊ฐ)`

```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
}
```

```css
.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
}
```

์์ ๋ ์ฝ๋๋ ๊ฐ์ ๊ฒฐ๊ณผ๋ฅผ ๋ํ๋ธ๋ค.

---

### 4-4. minmax ํจ์

์ต์๊ฐ๊ณผ ์ต๋๊ฐ์ ์ง์ ํ  ์ ์๋ ํจ์์ด๋ค.

`minmax(100px, auto)`์ ์๋ฏธ๋ '์ต์ํ 100px, ์ต๋๋ ์๋์ผ๋ก(auto) ๋์ด๋๊ฒ ํ๋ค.'์ด๋ค. ์๋ฌด๋ฆฌ ๋ด์ฉ์ ์์ด ์ ๋๋ผ๋ ์ต์ํ์ ๋์ด 100px์ ํ๋ณด๋๊ณ , ๋ด์ฉ์ด ๋ง์ 100px์ด ๋์ด๊ฐ๋ฉด ์์์ ๋์ด๋๋๋ก ํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container1 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: auto 1fr;
        margin-bottom: 20px;
      }
      .grid-container2 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(2, minmax(100px, auto));
      }
    </style>
  </head>
  <body>
    <div>
      grid-template-columns: repeat(3, 1fr); grid-template-rows: auto 1fr;
      <div class="grid-container1">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">
          Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi
          eius illo nisi id ut vel, neque aliquam sapiente ullam tempore beatae
          sit aperiam consequuntur! Soluta amet culpa ab omnis quo!
        </div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      grid-template-columns: repeat(3, 1fr); grid-template-rows: repeat(2,
      minmax(100px, auto));
      <div class="grid-container2">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">
          Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi
          eius illo nisi id ut vel, neque aliquam sapiente ullam tempore beatae
          sit aperiam consequuntur! Soluta amet culpa ab omnis quo!
        </div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![minmax](../image/CSS/Grid/gridMinmax.png)

---

### 4-5. auto-fill, auto-fit

`auto-fill`๊ณผ `auto-fit`์ column์ ๊ฐ์๋ฅผ ๋ฏธ๋ฆฌ ์ ํ์ง ์๊ณ  ์ค์ ๋ ๋๋น๊ฐ ํ์ฉํ๋ ํ ์ต๋ํ ์์ ์ฑ์ด๋ค.

`auto-fill`๊ณผ `auto-fit`์ ์ฐจ์ด๋ ๋จ๋ ๊ณต๊ฐ์ ์ฑ์ฐ์ง ์๋๋ ์ฑ์ฐ๋์ด๋ค. `auto-fill`์ ๋จ์ ๊ณต๊ฐ์ ์ฑ์ฐ์ง ์๊ณ  `auto-fit`์ ๋จ์ ๊ณต๊ฐ์ ์ฑ์ด๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container1 {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(25%, auto));
        margin-bottom: 20px;
      }
      .grid-container2 {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(25%, auto));
      }
    </style>
  </head>
  <body>
    <div>
      <div class="grid-container1">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      <div class="grid-container2">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![auto-fill, auto-fit](../image/CSS/Grid/autofill%3Afit.png)

> ๋ด๊ฐ ์๊ฐํ ๊ฒฐ๊ณผ๊ฐ ๋ค๋ฅด๋ค. ์๋ fill์ด๊ธฐ ๋๋ฌธ์ ์์๋ ๊ฒฐ๊ณผ์ด์ง๋ง ์๋๋ fit์ผ๋ก ์ ํด๋ 2๋ฒ์งธ ์ด์ด ๋ชจ๋  ๋๋น๋ฅผ ์ฐจ์งํ์ง ์๋๋ค. ์ค๊น?

---

## 5. ๊ฐ๊ฒฉ๋ง๋ค๊ธฐ

๊ทธ๋ฆฌ๋ ์ ์ฌ์ด์ ๊ฐ๊ฒฉ์ ์ค์ ์ ํ๊ธฐ ์ํด์๋ `row-gap`, `column-gap` ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ๋ค. ๊ฐ๊ฐ row(์ด), column(ํ)์ ๊ฐ๊ฒฉ์ ์ง์ ํ๋ฉฐ `gap` ํ๋กํผํฐ๋ `row-gap`๊ณผ `column-gap`๋ฅผ ์ง์ ํ  ์ ์๋ shorthand์ด๋ค.

์ฌ์ฉ๋ฒ `gap: row-gap, column-gap`
`row-gap`๊ณผ `column-gap`์ด ๊ฐ์ผ๋ฉด ํ๋๋ง ์ ์ด๋ ์ด๊ณผ ํ์ ๋ชจ๋ ์ ์ฉ์ด ๋๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 10px 0px;
        background-color: aliceblue;
      }
      .grid-container1 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        column-gap: 10px;
        row-gap: 10px;
        margin-bottom: 20px;
      }
      .grid-container2 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        column-gap: 30px;
        row-gap: 5px;
        margin-bottom: 20px;
      }
      .grid-container3 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 40px 10px;
        /* row-gap: 40px, column-gap: 10px*/
        margin-bottom: 20px;
      }
      .grid-container4 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 20px;
        /* row-gap: 20px, column-gap: 20px*/
      }
    </style>
  </head>
  <body>
    <div>
      column-gap: 10px; row-gap: 10px;
      <div class="grid-container1">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      column-gap: 30px; row-gap: 5px;
      <div class="grid-container2">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      gap: 40px 10px;
      <div class="grid-container3">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div>
      gap: 20px;
      <div class="grid-container4">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![gap](../image/CSS/Grid/gridgap.png)

---

## 6. ๊ทธ๋ฆฌ๋ ํํ๋ฅผ ์๋์ผ๋ก ์ ์

`grid-template-columns`๋๋ `grid-template-rows`์ **ํต์ ๋ฅผ ๋ฒ์ด๋** ์์น์ ์๋ ํธ๋์ ํฌ๊ธฐ๋ฅผ ์ง์ ํ๋ ์์ฑ์ด๋ค.

์๋์ ์ฝ๋๋ฅผ ๋ณด์.

```css
.grid-container {
  display: grid;
  grid-template-rows: repeat(3, minmax(320px, auto));
}
```

ํด๋น css๋ 3๋ฒ์งธ ์ด๊น์ง๋ง ํธ๋ ์ ํฌ๊ธฐ๋ฅผ ์ง์ ํ๊ณ  ์๋ค. ํ์ง๋ง ๋ง์ฝ 4๋ฒ์งธ ์ด์์ ์ด์ด ์๊ธธ ๊ฒฝ์ฐ ์ง์ ํด์ฃผ์ง ์์ ๋ค๋ฅธ ํฌ๊ธฐ์ ํธ๋ ์ด ์๊ธธ ์ ์๋ค. ์ด๋ 4๋ฒ์งธ ์ด์์ ์ด์ด ๋ฐ๋ก ํต์ ๋ฅผ ๋ฒ์ด๋ ์์น์ ์๋ ํธ๋ ์ด๋ค.
์ฆ, ์ฐ๋ฆฌ๊ฐ ์์ฑํ๊ณ ์ ํ๋ column๊ณผ row์ ๊ฐ์๋ฅผ ๋ชจ๋ฅผ ๋ `grid-template-columns`๋๋ `grid-template-rows` ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ๋ฉด ๋๋ค. ์ด๋ ๋ชจ๋  column๊ณผ row๋ ํต์ ๋ฅผ ๋ฒ์ด๋ ์์น์ ์๋ ํธ๋ ์ด๋ผ๊ณ  ํ  ์ ์๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-auto-rows: minmax(120px, auto);
      }
    </style>
  </head>
  <body>
    <div>
      <div class="grid-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">
          Lorem ipsum dolor sit amet consectetur, adipisicing elit. Omnis unde
          molestias sequi facere repudiandae architecto molestiae inventore
          ducimus rem, modi rerum, saepe reprehenderit tempore nemo minima ipsa
          laboriosam odit dolore?
        </div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![grid-auto-rows](../image/CSS/Grid/gridauto.png)

---

## 7. grid-column, grid-row

`grid-column`, `grid-row` ํ๋กํผํฐ๋ Grid ์์ดํ์ ์ ์ฉํ๋ ํ๋กํผํฐ๋ก, ๊ฐ ์์ ์์ญ์ ์ง์ ํ๋ค. Grid์๋ ์๋์ ๊ฐ์ ๋ฒํธ๊ฐ ๋งค๊ฒจ์ ธ ์๋ค.

![grid number](../image/CSS/Grid/gridNumber.png)

์ด๋ฌํ Grid ๋ฒํธ๋ฅผ ์ฌ์ฉํ์ฌ ์์ญ์ ์ง์ ํ๋ค.

์ฌ์ฉ๋ฒ `grid-column: <์์๋ฒํธ> / <๋๋ฒํธ>`  
์ฌ์ฉ๋ฒ `grid-row: <์์๋ฒํธ> / <๋๋ฒํธ>`

๋๋ ๋ช ๊ฐ์ ์์ ์ฐจ์งํ๊ฒ ํ  ๊ฒ์ด์ง๋ฅผ ์ง์ ํ  ์๋ ์๋ค.

์ฌ์ฉ๋ฒ `grid-column: <์์๋ฒํธ> / span<์์นํ  ์์ญ ์ ์>`  
์ฌ์ฉ๋ฒ `grid-row: <์์๋ฒํธ> / span<์์นํ  ์์ญ ์ ์>`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(120px, auto);
      }
      .box:nth-child(1) {
        grid-column: 1 / 4;
        grid-row: 1 / 3;
      }
      .box:nth-child(4) {
        grid-column: 1 / span 2;
      }
      .box:nth-child(8) {
        grid-column: 1 / -1;
        grid-row: 4 / 6;
      }
    </style>
  </head>
  <body>
    <div>
      <div class="grid-container">
        <div class="box">grid-column: 1 / 4; grid-row: 1 / 3;</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">grid-column: 1 / span 2;</div>
        <div class="box">5</div>
        <div class="box">6</div>
        <div class="box">7</div>
        <div class="box">grid-column: 1 / -1; grid-row: 4 / 6;</div>
        <div class="box">9</div>
        <div class="box">10</div>
      </div>
    </div>
  </body>
</html>
```

![grid-column, grid-row](../image/CSS/Grid/girdcolumnrow.png)

---

## 8. ์ธ๋ก ๋ฐฉํฅ ์ ๋ ฌ

### 8-1. align-items

`align-items`๋ ์ปจํ์ด๋์ ์ ์ฉํ๋ ํ๋กํผํฐ๋ก ์์ดํ๋ค์ ์ธ๋ก(column์ถ) ๋ฐฉํฅ์ผ๋ก ์ ๋ ฌํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container {
        height: 300px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
      }
      .stretch {
        align-items: stretch;
      }
      .start {
        align-items: start;
      }
      .end {
        align-items: end;
      }
      .center {
        align-items: center;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      align-items: stretch;
      <div class="grid-container stretch">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-items: start;
      <div class="grid-container start">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-items: end;
      <div class="grid-container end">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-items: center;
      <div class="grid-container center">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![align-items](../image/CSS/Grid/alignItems1.png)
![align-items](../image/CSS/Grid/alignItems2.png)

---

### 8-2. align-content

`align-content`๋ ์ปจํ์ด๋์ ์ ์ฉํ๋ ํ๋กํผํฐ๋ก Gird ์์ดํ๋ค์ ๋์ด๋ฅผ ๋ชจ๋ ํฉํ ๊ฐ์ด Grid ์ปจํ์ด๋์ ๋์ด๋ณด๋ค ์์ ๋ Grid ์์ดํ๋ค์ ํต์งธ๋ก ์ ๋ ฌํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container {
        height: 200px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
      }
      .start {
        align-content: start;
      }
      .end {
        align-content: end;
      }
      .center {
        align-content: center;
      }
      .space-between {
        align-content: space-between;
      }
      .space-around {
        align-content: space-around;
      }
      .space-evenly {
        align-content: space-evenly;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      align-content: start;
      <div class="grid-container start">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-content: end;
      <div class="grid-container end">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-content: center;
      <div class="grid-container center">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-content: space-between;
      <div class="grid-container space-between">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-content: space-around;
      <div class="grid-container space-around">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      align-content: space-evenly;
      <div class="grid-container space-evenly">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![align-content](../image/CSS/Grid/alignContent1.png)
![align-content](../image/CSS/Grid/alignContent2.png)

---

### 8-3. align-self

`align-self`๋ Grid ์์ดํ์ ์ ์ฉํ๋ ํ๋กํผํฐ๋ก ํด๋น ์์ดํ์ ์ธ๋ก(column์ถ) ๋ฐฉํฅ์ผ๋ก ์ ๋ ฌํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 0px;
        background-color: aliceblue;
      }
      .grid-container {
        height: 300px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-auto-rows: 1fr;
      }
      .box:nth-child(1) {
        align-self: stretch;
      }
      .box:nth-child(2) {
        align-self: start;
      }
      .box:nth-child(5) {
        align-self: center;
      }
      .box:nth-child(6) {
        align-self: end;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      <div class="grid-container start">
        <div class="box">align-self: stretch;</div>
        <div class="box">align-self: start;</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">align-self: center;</div>
        <div class="box">align-self: end;</div>
      </div>
    </div>
  </body>
</html>
```

![align-self](../image/CSS/Grid/alignSelf.png)

---

## 9. ๊ฐ๋ก ๋ฐฉํฅ ์ ๋ ฌ

### 9-1. justify-items

`justify-items` ํ๋กํผํฐ๋ Gird ์ปจํ์ด๋์ ์ ์ฉํ๋ฉฐ ์์ดํ๋ค์ ๊ฐ๋ก(row์ถ) ๋ฐฉํฅ์ผ๋ก ์ ๋ ฌํ  ๋ ์ฌ์ฉํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 40px;
        background-color: aliceblue;
      }
      .grid-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
      }
      .start {
        justify-items: start;
      }
      .end {
        justify-items: end;
      }
      .center {
        justify-items: center;
      }
      .stretch {
        justify-items: stretch;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      justify-items: start;
      <div class="grid-container start">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-items: end;
      <div class="grid-container end">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-items: center;
      <div class="grid-container center">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-items: stretch;
      <div class="grid-container stretch">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![justify-items](../image/CSS/Grid/justifyItems.png)

---

### 9-2. justify-content

`justify-content` ํ๋กํผํฐ๋ Grid ์ปจํ์ด๋์ ์ ์ฉํ๋ฉฐ Grid ์์ดํ๋ค์ ๋๋น๋ฅผ ๋ชจ๋ ํฉํ ๊ฐ์ด Grid ์ปจํ์ด๋์ ๋๋น๋ณด๋ค ์์ ๋ Grid ์์ดํ๋ค์ ํต์งธ๋ก ์ ๋ ฌํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        width: 200px;
        text-align: center;
        padding: 20px 40px;
        background-color: aliceblue;
      }
      .grid-container {
        display: grid;
        grid-template-columns: repeat(3, auto);
      }
      .stretch {
        justify-content: stretch;
      }
      .start {
        justify-content: start;
      }
      .center {
        justify-content: center;
      }
      .end {
        justify-content: end;
      }
      .space-between {
        justify-content: space-between;
      }
      .space-around {
        justify-content: space-around;
      }
      .space-evenly {
        justify-content: space-evenly;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      justify-content: stretch;
      <div class="grid-container stretch">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-content: start;
      <div class="grid-container start">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-content: center;
      <div class="grid-container center">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-content: end;
      <div class="grid-container end">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-content: space-between;
      <div class="grid-container space-between">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-content: space-around;
      <div class="grid-container space-around">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
    <div class="parents">
      justify-content: space-evenly;
      <div class="grid-container space-evenly">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
      </div>
    </div>
  </body>
</html>
```

![justify-content](../image/CSS/Grid/justifyContent1.png)
![justify-content](../image/CSS/Grid/justifyContent2.png)

---

### 9-3. justify-self

`justify-self`๋ Grid ์์ดํ์ ์ ์ฉํ๋ ํ๋กํผํฐ๋ก ํด๋น ์์ดํ์ ๊ฐ๋ก(row์ถ) ๋ฐฉํฅ์ผ๋ก ์ ๋ ฌํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 40px;
        background-color: aliceblue;
      }
      .grid-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
      }
      .box:nth-child(1) {
        justify-self: stretch;
      }
      .box:nth-child(2) {
        justify-self: start;
      }
      .box:nth-child(5) {
        justify-self: center;
      }
      .box:nth-child(6) {
        justify-self: end;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      <div class="grid-container">
        <div class="box">justify-self: stretch;</div>
        <div class="box">justify-self: start;</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">justify-self: center;</div>
        <div class="box">justify-self: end;</div>
      </div>
    </div>
  </body>
</html>
```

![justify-self](../image/CSS/Grid/justifySelf.png)

---

## 10. ์ ๋ ฌ shorthand

### 10-1. place-content

`align-content`์ `justify-content`๋ฅผ ๊ฐ์ด ์ธ ์ ์๋ shorthand์ด๋ค.

์ฌ์ฉ๋ฒ `place-content: <align-content> <justify-content>`

ํ๋์ ๊ฐ๋ง ์ฐ๋ฉด ๋ ํ๋กํผํฐ ๋ชจ๋์ ์ ์ฉ๋๋ค.

---

### 10-2. place-slef

`align-slef`์ `justify-slef`๋ฅผ ๊ฐ์ด ์ธ ์ ์๋ shorthand์ด๋ค.

์ฌ์ฉ๋ฒ `place-slef: <align-slef> <justify-slef>`

ํ๋์ ๊ฐ๋ง ์ฐ๋ฉด ๋ ํ๋กํผํฐ ๋ชจ๋์ ์ ์ฉ๋๋ค.

---

## 11. order

๊ฐ Grid ์์ดํ๋ค์ ์๊ฐ์  ๋์ด ์์๋ฅผ ๊ฒฐ์ ํ๋ ์์ฑ์ด๋ค.

์ซ์๊ฐ์ด ๋ค์ด๊ฐ๋ฉด, ์์ ์ซ์์ผ ์๋ก ๋จผ์  ๋ฐฐ์น๋๋ค. "์๊ฐ์ " ์์์ผ ๋ฟ HTML ์์ฒด์ ๊ตฌ์กฐ๋ฅผ ๋ฐ๊พธ์ง ์๋ค.

---

## 12. z-index

Z์ถ ์ ๋ ฌ์ ํ  ์ ์๋ ํ๋กํผํฐ์ด๋ค. ์ซ์๊ฐ ํด ์๋ก ์๋ก ์ฌ๋ผ๊ฐ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 24px;
        box-sizing: border-box;
      }
      .parents {
        background-color: greenyellow;
        padding: 5px;
        margin-bottom: 20px;
      }
      .box {
        border: 2px solid black;
        border-radius: 10px;
        text-align: center;
        padding: 20px 40px;
        background-color: aliceblue;
      }
      .grid-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
      }
      .box:nth-child(5) {
        z-index: 1;
        background-color: burlywood;
        opacity: 0.8;
        transform: scale(2);
      }
    </style>
  </head>
  <body>
    <div class="parents">
      <div class="grid-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
        <div class="box">7</div>
        <div class="box">8</div>
        <div class="box">9</div>
      </div>
    </div>
  </body>
</html>
```

![z-index](../image/CSS/Grid/zIndex.png)

---

## 13. Conclusion

> ๋ด๊ฐ ์๋ฐ์คํฌ๋ฆฝํธ์ ์น ํ๋ ์์ํฌ/๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ค React๋ฅผ ์ฌ๋ํ๋ ๊ฒ๊ณผ ๊ฐ์ด CSS์์๋ Grid๋ฅผ ๊ฐ์ฅ ๋ง์ด ์ฌ๋ํ๋ค. ๊ทธ ๋งํผ ์ฌ์ฉํ๋ ๋น์ค์ด ๋ง๊ณ  ์ ์ฉํ๊ฒ ์ฌ์ฉํ๊ณ  ์๋ค๋ ๋ป์ด๋ค. ํ์คํ Grid๋ฅผ ์ฒ์ ๊ณต๋ถํ  ๋ ํ๋ํ๋ ์ดํดํ๋ ค๊ณ  ๋ง์ ์๊ฐ์ ๋ณด๋๋๋ฐ ์ง๊ธ์ Flex์ ํจ๊ป ์๋ก ๋น๊ตํ๋ฉฐ ๊ณต๋ถํ๋ ์ฝ๊ฒ ์ดํด๊ฐ ๋์๋ค. ๋ค๋ง ์ฃผ๋ก ์ฌ์ฉํ์ง ์๋ ํ๋กํผํฐ๊ฐ ์๋๋ฐ ๋ฐ๋ก `justify-content`์ `align-content`์ด๋ค. ์ด ๋ ์์ฑ์ Flex์์๋ ๋ง์ด ์ฌ์ฉํ์ง๋ง Grid์์๋ ํ ๋ฒ๋ ์ฌ์ฉํ์ง ์์๋ ํ๋กํผํฐ์ธ๊ฑฐ ๊ฐ๋ค. ๊ทธ๋๋ ์ด๋ค ํน์ง์ ๊ฐ์ง๊ณ  ์๋์ง ์ง๊ณ  ๋์ด๊ฐ์.  
> ๊ทธ๋ฆฌ๊ณ  ์ฐ์ฐํ๊ฒ.. `auto-fill`, `auto-fit`์ด ๋ด ์๊ฐ๋๋ก, ์์ํ๋๋ก ์๋ํ์ง ์์์ ์กฐ๊ธ์ ์์ธํ๋ค... ํด๊ฒฐ์ฑ์ ์ฐพ์๋ด์ผ๊ฒ ๋ค.๐ญ

---

## ์ฐธ๊ณ 

[The CSS Grid Enchiridion](https://medium.com/stephenkoo/the-css-grid-enchiridion-1ca2d9fd68fe)  
[์ด๋ฒ์์ผ๋ง๋ก CSS Grid๋ฅผ ์ตํ๋ณด์](https://studiomeal.com/archives/533)  
[Understanding CSS Grid: Grid Lines](https://www.smashingmagazine.com/2020/01/understanding-css-grid-lines/)

---

[๐](#grid-layout)
