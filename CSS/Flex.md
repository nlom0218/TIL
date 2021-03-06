# Flexbox Layout

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. ์ฉ์ด ์ ๋ฆฌ](#2-์ฉ์ด-์ ๋ฆฌ)
- [3. Usage](#3-usage)
- [4. Flexbox container ์์ฑ](#4-flexbox-container-์์ฑ)
  - [4.1 flex-direction](#41-flex-direction)
  - [4-2 flex-wrap](#4-2-flex-wrap)
  - [4-3. flex-flow](#4-3-flex-flow)
  - [4-4 justify-content](#4-4-justify-content)
  - [4-5 align-items](#4-5-align-items)
  - [4-6. align-content](#4-6-align-content)
- [5. Flexbox item ์์ฑ](#5-flexbox-item-์์ฑ)
  - [5-1. order](#5-1-order)
  - [5-2. flex-grow](#5-2-flex-grow)
  - [5-3. flex-shrink](#5-3-flex-shrink)
  - [5-4. flex-basis](#5-4-flex-basis)
  - [5-5. flex](#5-5-flex)
  - [5-6. align-self](#5-6-align-self)
- [6. Conclusion](#6-conclusion)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

Flexbox Layout์ ๋ชจ๋ ์น์ ์ํ์ฌ ์ ์๋ ๊ธฐ์กด layout๋ณด๋ค ๋ ์ธ๋ จ๋ ๋ฐฉ์์ ๋์ฆ์ ๋ถํฉํ๊ธฐ ์ํ CSS3์ ์๋ก์ด layout๋ฐฉ์์ด๋ค. ํ๋์ค ๋ฐ์ค ๋ ์ด์์์ ๋น๊ต์  ์ต๊ทผ์ ๋ฑ์ฅํ ๊ฐ๋์ด๋ฏ๋ก ๊ธฐ์กด์ CSS ์์ฑ ์ธ์ ์๋ก ๊ณต๋ถํด์ผ ํ  ์์ฑ์ด ์๋ค.

---

## 2. ์ฉ์ด ์ ๋ฆฌ

![Flex Layout Box Model](../image/CSS/Flex/flexboxlayoutmodel.png)

- ํ๋ ์ค ์ปจํ์ด๋(๋ถ๋ชจ ๋ฐ์ค): ํ๋ ์ค ๋ฐ์ค ๋ ์ด์์์ ์ ์ฉํ  ๋์์ ๋ฌถ๋ ์์์ด๋ค. ์์ ๊ทธ๋ฆผ์์๋ ๋ธ๋ผ์ ๋ฐฐ๊ฒฝ์ ๊ฐ์ง ํฐ ๋ฐ์ค๊ฐ ํ๋ ์ค ์ปจํ์ด๋์ ํด๋น๋๋ค.
- ํ๋ ์ค ํญ๋ชฉ(์์ ๋ฐ์ค, flex-item): ํ๋ ์ค ๋ฐ์ค ๋ ์ด์์์ ์ ์ฉํ  ๋์์ด๋ค. ์์ ๊ทธ๋ฆผ์์๋ ํ์์ ๋ฐฐ๊ฒฝ์ ๊ฐ์ง ์์ ๋ฐ์ค๊ฐ ํ๋ ์ค ํญ๋ชฉ์ ํด๋น๋๋ค.
- ์ฃผ์ถ(main axis): ํ๋ ์ค ์ปจํ์ด๋ ์์์ ํ๋ ์ค ํญ๋ชฉ์ ๋ฐฐ์นํ๋ ๊ธฐ๋ณธ ๋ฐฉํฅ์ด๋ค. ๊ธฐ๋ณธ์ ์ผ๋ก ์ผ์ชฝ์์ ์ค๋ฅธ์ชฝ์ด๋ฉฐ ์ํ ๋ฐฉํฅ์ผ๋ก ๋ฐฐ์นํ๋ค. ํ๋ ์ค ํญ๋ชฉ์ ๋ฐฐ์น๊ฐ ์์๋๋ ์์น๋ฅผ '์ฃผ์ถ ์์์ ', ๋๋๋ ์์น๋ฅผ '์ฃผ์ถ ๋์ '์ด๋ผ๊ณ  ํ๋ค.
- ๊ต์ฐจ์ถ(cross axis): ์ฃผ์ถ๊ณผ ๊ต์ฐจํ๋ ๋ฐฉํฅ์ ๋งํ๋ฉฐ ๊ธฐ๋ณธ์ ์ผ๋ก ์์์ ์๋๋ก ๋ฐฐ์นํ๋ค. ํ๋ ์ค ํญ๋ชฉ์ ๋ฐฐ์น๊ฐ ์์๋๋ ์์น๋ฅผ '๊ต์ฐจ์ถ ์์์ ', ๋๋๋ ์์น๋ฅผ '๊ต์ฐจ์ถ ๋์ '์ด๋ผ๊ณ  ํ๋ค.

---

## 3. Usage

ํ๋ ์ค ๋ฐ์ค ๋ ์ด์์์ ๋ง๋ค๊ธฐ ์ํด์๋ ๋จผ์  ์น ์ฝํ์ธ ๋ฅผ ํ๋ ์ค ์ปจํ์ด๋๋ก ๋ฌถ์ด ์ฃผ์ด์ผ ํ๋ค. ์ฆ, ๋ฐฐ์นํ  ์น ์์๊ฐ ์๋ค๋ฉด ๊ทธ ์์๋ฅผ ๊ฐ์ธ๋ ๋ถ๋ชจ ์์๋ฅผ ๋ง๋ค๊ณ , ๊ทธ ๋ถ๋ชจ ์์๋ฅผ ํ๋ ์ค ์ปจํ์ด๋๋ก ๋ง๋ค์ด์ผ ํ๋ค.

๋ถ๋ชจ ์์์ display ์์ฑ์ flex๋ฅผ ์ง์ ํ๋ฉด ํ๋ ์ค ์ปจํ์ด๋๊ฐ ๋ง๋ค์ด์ง๋ค.

```css
.flex-container {
  display: flex;
}
```

๋ถ๋ชจ ์์๊ฐ inline ์์์ธ ๊ฒฝ์ฐ inline-flex์ ์ง์ ํ๋ค.

```css
.flex-container {
  display: inline-flex;
}
```

Flexbox Layout๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด์๋ flex ๋๋ inline-flex๋ ๋ถ๋ชจ ์์์ ๋ฐ๋์ ์ง์ ํด์ผํ๋ฉฐ ์์ ์์๋ ์๋์ ์ผ๋ก flex item์ด ๋๋ค.

---

## 4. Flexbox container ์์ฑ

### 4.1 flex-direction

ํ๋ ์ค ์ปจํ์ด๋ ์์์ ํ๋ ์ค ํญ๋ชฉ์ ๋ฐฐ์นํ๋ ์ฃผ์ถ(main axis) ๋ฐฉํฅ์ ์ค์ ํ๋ค.

์๋๋ `flex-direction`์ ํ๋กํผํฐ๊ฐ๊ณผ ์ค๋ช์ด๋ค.

| ์ข๋ฅ           | ์ค๋ช                                                         |
| :------------- | :----------------------------------------------------------- |
| row            | ์ฃผ์ถ์ ๊ฐ๋ก๋ก ์ง์ ํ๊ณ  ์ผ์ชฝ์์ ์ค๋ฅธ์ชฝ์ผ๋ก ๋ฐฐ์นํ๋ค.(๊ธฐ๋ณธ๊ฐ) |
| row-reverse    | ์ฃผ์ถ์ ๊ฐ๋ก๋ก ์ง์ ํ๊ณ  ๋ฐ๋๋ก ์ค๋ฅธ์ชฝ์์ ์ผ์ชฝ์ผ๋ก ๋ฐฐ์นํ๋ค.  |
| column         | ์ฃผ์ถ์ ์ธ๋ก๋ก ์ง์ ํ๊ณ  ์์ชฝ์์ ์๋์ชฝ์ผ๋ก ๋ฐฐ์นํ๋ค.         |
| column-reverse | ์ฃผ์ถ์ ์ธ๋ก๋ก ์ง์ ํ๊ณ  ์๋์ชฝ์์ ์์ชฝ์ผ๋ก ๋ฐฐ์นํ๋ค.         |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 20px;
        font-weight: 700;
      }
      .box {
        background-color: yellow;
        width: 60px;
        height: 60px;
        text-align: center;
        line-height: 60px;
        margin: 10px;
      }
      .row {
        display: flex;
        flex-direction: row;
      }
      .row-reverse {
        display: flex;
        flex-direction: row-reverse;
      }
      .column {
        display: flex;
        flex-direction: column;
      }
      .column-reverse {
        display: flex;
        flex-direction: column-reverse;
      }
    </style>
  </head>
  <body>
    <div>
      row
      <div class="row">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
      </div>
    </div>
    <div>
      row-reverse
      <div class="row-reverse">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
      </div>
    </div>
    <div>
      column
      <div class="column">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
      </div>
    </div>
    <div>
      column-reverse
      <div class="column-reverse">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
      </div>
    </div>
  </body>
</html>
```

![flex-direction](../image/CSS/Flex/flexDirection.png)

---

### 4-2 flex-wrap

`flex-wrap` ํ๋กํผํฐ๋ ํ๋ ์ค ์ปจํ์ด๋ ๋๋น๋ณด๋ค ๋ง์ ํ๋ ์ค ํญ๋ชฉ์ด ์์ ๊ฒฝ์ฐ ์ค์ ๋ฐ๊ฟ์ง ์ฌ๋ถ๋ฅผ ์ง์ ํ๋ค.

์๋๋ `flex-wrap`์์ ์ฌ์ฉํ  ์ ์๋ ํ๋กํผํฐ๊ฐ๊ณผ ์ค๋ช์ด๋ค.

| ์ข๋ฅ         | ์ค๋ช                                                      |
| :----------- | :-------------------------------------------------------- |
| nowrap       | ํ๋ ์ค ํญ๋ชฉ์ ํ ์ค์ ํ์ํ๋ค.(๊ธฐ๋ณธ๊ฐ)                   |
| warp         | ํ๋ ์ค ํญ๋ชฉ์ ์ฌ๋ฌ ์ค์ ํ์ํ๋ค.                         |
| wrap-reverse | ํ๋ ์ค ํญ๋ชฉ์ ์ฌ๋ฌ ์ค์ ํ์ํ๋, ์์์ ๊ณผ ๋์ ์ด ๋ฐ๋๋ค. |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 20px;
        font-weight: 700;
      }
      .box {
        background-color: yellow;
        width: 200px;
        height: 60px;
        text-align: center;
        line-height: 60px;
        margin: 10px;
      }
      .nowrap {
        display: flex;
        flex-wrap: nowrap;
      }
      .wrap {
        display: flex;
        flex-wrap: wrap;
      }
      .wrap-reverse {
        display: flex;
        flex-wrap: wrap-reverse;
      }
    </style>
  </head>
  <body>
    <div>
      nowrap
      <div class="nowrap">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
        <div class="box">7</div>
      </div>
    </div>
    <div>
      wrap
      <div class="wrap">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
        <div class="box">7</div>
      </div>
    </div>
    <div>
      wrap-reverse
      <div class="wrap-reverse">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
        <div class="box">7</div>
      </div>
    </div>
  </body>
</html>
```

![flex-wrap](../image/CSS/Flex/flexWrap.png)

---

### 4-3. flex-flow

`flex-flow` ํ๋กํผํฐ๋ `flex-direction` ํ๋กํผํฐ์ `flex-wrap` ํ๋กํผํฐ๋ฅผ ๋์์ ์ค์ ํ๊ธฐ ์ํ shorthand์ด๋ค. ๊ธฐ๋ณธ๊ฐ์ `row nowarp`์ด๋ค.

์ฌ์ฉ๋ฒ `flex-flow: <flex-direction> || <flex-wrap>;`

---

### 4-4 justify-content

`justify-content` ํ๋กํผํฐ๋ ์ฃผ์ถ(main axis)์ ๊ธฐ์ค์ผ๋ก flex item์ ์ํ ์ ๋ ฌ ๋ฐฉ๋ฒ์ ์ง์ ํ๋ค.

> `flex-direction: column;`์ผ๋ก ์ธํด ์ฃผ์ถ์ด ๋ฐ๋๊ฒ ๋๋ ๊ฒฝ์ฐ๋ฅผ ์กฐ์ฌํ์.

์๋๋ `justify-content`์ ํ๋กํผํฐ๊ฐ๊ณผ ์ค๋ช์ด๋ค.

| ์ข๋ฅ          | ์ค๋ช                                                                                                      |
| :------------ | :-------------------------------------------------------------------------------------------------------- |
| flex-start    | ์ฃผ์ถ์ ์์์ ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                                                                            |
| flex-end      | ์ฃผ์ถ์ ๋์ ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                                                                              |
| center        | ์ฃผ์ถ์ ์ค์์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                                                                              |
| space-between | ์ฒซ ๋ฒ์ฌ ํญ๋ชฉ๊ณผ ๋ ํญ๋ชฉ์ ์ฃผ์ถ์ ์์์ ๊ณผ ๋์ ์ ๋ฐฐ์นํ ํ ๋๋จธ์ง ํญ๋ชฉ์ ๊ทธ ์ฌ์ด์ ๊ฐ์ ๊ฐ๊ฒฉ์ผ๋ก ๋ฐฐ์นํ๋ค. |
| space-around  | ๋ชจ๋  ํญ๋ชฉ์ ์ฃผ์ถ์ ๊ฐ์ ๊ฐ๊ฒฉ์ผ๋ก ๋ฐฐ์นํ๋ค.                                                                |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 20px;
        font-weight: 700;
      }
      .parents {
        margin-bottom: 10px;
      }
      .flex-container {
        background-color: green;
      }
      .box {
        background-color: yellow;
        width: 200px;
        height: 60px;
        text-align: center;
        line-height: 60px;
        margin: 10px;
      }
      .flex-start {
        display: flex;
        justify-content: flex-start;
      }
      .flex-end {
        display: flex;
        justify-content: flex-end;
      }
      .center {
        display: flex;
        justify-content: center;
      }
      .space-between {
        display: flex;
        justify-content: space-between;
      }
      .space-around {
        display: flex;
        justify-content: space-around;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      flex-start
      <div class="flex-start flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      flex-end
      <div class="flex-end flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      center
      <div class="center flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      space-between
      <div class="space-between flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      space-around
      <div class="space-around flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
  </body>
</html>
```

## ![justify-content](../image/CSS/Flex/justifyContent.png)

---

### 4-5 align-items

`align-items` ํ๋กํผํฐ๋ ์์ง ๋ฐฉํฅ(cross axis)์ ๊ธฐ์ค์ผ๋ก flex item์ ์ ๋ ฌํ๋ค. `align-items` ํ๋กํผํฐ๋ ๋ชจ๋  flex-item์ ์ ์ฉ๋๋ค.

์๋๋ `align-items`์ ํ๋กํผํฐ๊ฐ๊ณผ ์ค๋ช์ด๋ค.

| ์ข๋ฅ       | ์ค๋ช                                            |
| :--------- | :---------------------------------------------- |
| flex-start | ๊ต์ฐจ์ถ์ ์์์ ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                |
| flex-end   | ๊ต์ฐจ์ถ์ ๋์ ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                  |
| center     | ๊ต์ฐจ์ถ์ ์ค์์ ๋ฐฐ์นํ๋ค.                       |
| baseline   | ๊ต์ฐจ์ถ์ ๋ฌธ์ ๊ธฐ์ค์ ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.           |
| stretch    | ํ๋ ์ค ํญ๋ชฉ์ ๋๋ ค ๊ต์ฐจ์ถ์ ๊ฐ๋ ์ฐจ๊ฒ ๋ฐฐ์นํ๋ค. |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 20px;
        font-weight: 700;
      }
      .parents {
        margin-bottom: 10px;
        background-color: green;
        color: white;
      }
      .flex-container {
        min-height: 120px;
        border-top: 2px solid black;
        padding: 0;
        margin: 0;
        color: green;
      }
      .box {
        background-color: yellow;
        width: 200px;
        text-align: center;
        line-height: 60px;
        margin: 0px 10px;
      }
      .flex-start {
        display: flex;
        align-items: flex-start;
      }
      .flex-end {
        display: flex;
        align-items: flex-end;
      }
      .center {
        display: flex;
        align-items: center;
      }
      .baseline {
        display: flex;
        align-items: baseline;
      }
      .stretch {
        display: flex;
        align-items: stretch;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      flex-start
      <div class="flex-start flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      flex-end
      <div class="flex-end flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      center
      <div class="center flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      baseline
      <div class="baseline flex-container">
        <div class="box" style="padding-top: 10px">1</div>
        <div class="box" style="height: 80px">2</div>
        <div class="box" style="font-size: 32px">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      stretch
      <div class="stretch flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
  </body>
</html>
```

![align-items](../image/CSS/Flex/aligenItems.png)

---

### 4-6. align-content

flex container์ cross axis๋ฅผ ๊ธฐ์ค์ผ๋ก flex item์ ์์ง ์ ๋ ฌํ๋ค.
์ฐธ๊ณ ๋ก `justify-content` ํ๋กํผํฐ๋ flex container์ main aixs๋ฅผ ๊ธฐ์ค์ผ๋ก flex item์ ์ํ ์ ๋ ฌํ๋ค.

์ฆ, ์ฃผ์ถ์์ ์ค ๋ฐ๊ฟ์ด ์๊ฒจ์ flex item์ ์ฌ๋ฌ ์ค๋ก ํ์ํ  ๋ `align-content` ํ๋กํผํฐ๋ฅผ ํตํด ๊ต์ฐจ์ถ์์ flex item์ ๊ฐ๊ฒฉ์ ์ง์ ํ  ์ ์๋ค.

์๋๋ `align-content`์ ํ๋กํผํฐ๊ฐ๊ณผ ์ค๋ช์ด๋ค.

| ์ข๋ฅ          | ์ค๋ช                                                                                                     |
| :------------ | :------------------------------------------------------------------------------------------------------- |
| flex-start    | ๊ต์ฐจ์ถ์ ์์์ ์์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                                                                       |
| flex-end      | ๊ต์ฐจ์ถ์ ๋์ ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                                                                           |
| center        | ๊ต์ฐจ์ถ์ ์ค์์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค.                                                                           |
| space-between | ์ฒซ ๋ฒ์งธ ํญ๋ชฉ๊ณผ ๋ ํญ๋ชฉ์ ๊ต์ฐจ์ถ์ ์์์ ๊ณผ ๋์ ์ ๋ง์ถ๊ณ  ๋๋จธ์ง ํญ๋ชฉ์ ๊ทธ ์ฌ์ด์ ๊ฐ์ ๊ฐ๊ฒฉ์ผ๋ก ๋ฐฐ์นํ๋ค. |
| space-around  | ๋ชจ๋  ํญ๋ชฉ์ ๊ต์ฐจ์ถ์ ๊ฐ์ ๊ฐ๊ฒฉ์ผ๋ก ๋ฐฐ์นํ๋ค.                                                             |
| stretch       | ํ๋ ์ค ํญ๋ชฉ์ ๋๋ ค์ ๊ต์ฐจ์ถ์ ๊ฐ๋ ์ฐจ๊ฒ ๋ฐฐ์นํ๋ค.                                                        |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        font-size: 20px;
        font-weight: 700;
      }
      .parents {
        margin-bottom: 10px;
        background-color: green;
        color: white;
      }
      .flex-container {
        min-height: 160px;
        border-top: 2px solid black;
        padding: 0;
        margin: 0;
        color: green;
      }
      .box {
        background-color: yellow;
        width: 200px;
        text-align: center;
        line-height: 60px;
        margin: 0px 10px;
      }
      .box:nth-child(1) {
        margin-bottom: 10px;
      }
      .box:nth-child(2) {
        margin-bottom: 10px;
      }
      .flex-start {
        display: flex;
        flex-wrap: wrap;
        align-content: flex-start;
      }
      .flex-end {
        display: flex;
        flex-wrap: wrap;
        align-content: flex-end;
      }
      .center {
        display: flex;
        flex-wrap: wrap;
        align-content: center;
      }
      .sapce-between {
        display: flex;
        flex-wrap: wrap;
        align-content: space-between;
      }
      .space-around {
        display: flex;
        flex-wrap: wrap;
        align-content: space-around;
      }
      .stretch {
        display: flex;
        flex-wrap: wrap;
        align-content: stretch;
      }
    </style>
  </head>
  <body>
    <div class="parents">
      flex-start
      <div class="flex-start flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      flex-end
      <div class="flex-end flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      center
      <div class="center flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      sapce-between
      <div class="sapce-between flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      space-around
      <div class="space-around flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
    <div class="parents">
      stretch
      <div class="stretch flex-container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
      </div>
    </div>
  </body>
</html>
```

![align-content1](../image/CSS/Flex/AlignContent1.png)
![align-content2](../image/CSS/Flex/AlignContent2.png)

---

## 5. Flexbox item ์์ฑ

### 5-1. order

flex item์ ๋ฐฐ์น ์์๋ฅผ ์ง์ ํ๋ค. HTML ์ฝ๋๋ฅผ ๋ณ๊ฒฝํ์ง ์๊ณ  order ํ๋กํผํฐ๊ฐ์ ์ง์ ํ๋ ๊ฒ์ผ๋ก ๊ฐ๋จํ ์ฌ๋ฐฐ์นํ  ์ ์๋ค. ๊ธฐ๋ณธ ๋ฐฐ์น ์์๋ flex container์ ์ถ๊ฐ๋ ์์์ด๋ค. ๊ธฐ๋ณธ๊ฐ์ 0์ด๋ค.

์ฌ์ฉ๋ฒ `order: <์ ์๊ฐ>`

```css
.flex-item {
  order: 2;
}
```

---

### 5-2. flex-grow

flex item์ ๋๋น์ ๋ํ ํ๋ ์ธ์๋ฅผ ์ง์ ํ๋ค. ๊ธฐ๋ณธ๊ฐ์ 0์ด๊ณ  ์์๊ฐ์ ๋ฌดํจํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .flex-container {
        display: flex;
        justify-content: space-around;
      }
      .box {
        height: 120px;
        background-color: yellow;
      }
      .box:not(:last-child) {
        margin-right: 10px;
      }
      .box:not(:nth-child(2)) {
        flex-grow: 1;
      }
      .box:nth-child(2) {
        flex-grow: 3;
      }
    </style>
  </head>
  <body>
    <div class="flex-container">
      <div class="box">1</div>
      <div class="box">2 / flex-grow: 3</div>
      <div class="box">3</div>
      <div class="box">4</div>
    </div>
  </body>
</html>
```

![flex-grow](../image/CSS/Flex/flexGrow.png)

---

### 5-3. flex-shrink

flex item์ ๋๋น์ ๋ํ ์ถ์ ์ธ์๋ฅผ ์ง์ ํ๋ค. ๊ธฐ๋ณธ๊ฐ์ 1์ด๊ณ  ์์๊ฐ์ ๋ฌดํจํ๋ค. 0๋ฅผ ์ง์ ํ๋ฉด ์ถ์๊ฐ ํด์ ๋์ด ์๋์ ๋๋น๋ฅผ ์ ์งํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .flex-container {
        display: flex;
        justify-content: space-around;
      }
      .box {
        width: 300px;
        height: 120px;
        background-color: yellow;
      }
      .box:not(:last-child) {
        margin-right: 10px;
      }
      .box:nth-child(2) {
        flex-shrink: 0;
      }
      .box:nth-child(3) {
        flex-shrink: 2;
      }
    </style>
  </head>
  <body>
    <div class="flex-container">
      <div class="box">1</div>
      <div class="box">2 / flex-shrink: 0</div>
      <div class="box">3 / flex-shrink: 2</div>
      <div class="box">4</div>
    </div>
  </body>
</html>
```

![flex-shrink](../image/CSS/Flex/flexShrink.png)

---

### 5-4. flex-basis

flex item์ ๋๋น ๊ธฐ๋ณธ๊ฐ์ px, % ๋ฑ์ ๋จ์๋ก ์ง์ ํ๋ค. ๊ธฐ๋ณธ๊ฐ์ auto์ด๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .flex-container {
        display: flex;
        justify-content: space-around;
      }
      .box {
        width: 100px;
        height: 120px;
        background-color: yellow;
      }
      .box:not(:last-child) {
        margin-right: 10px;
      }
      .box:nth-child(2) {
        flex-basis: 500px;
      }
      .box:nth-child(3) {
        flex-basis: 50%;
      }
    </style>
  </head>
  <body>
    <div class="flex-container">
      <div class="box">1 / flex-basis: auto => 100px</div>
      <div class="box">2 / flex-basis: 500px</div>
      <div class="box">3 / flex-basis: 50%</div>
      <div class="box">4 / flex-basis: auto => 100px</div>
    </div>
  </body>
</html>
```

![flex-basis](../image/CSS/Flex/flexBasis.png)

---

### 5-5. flex

`flex-grow`, `flex-shrink`, `flex-basis` ํ๋กํผํฐ์ shorthand์ด๋ค. ๊ธฐ๋ณธ๊ฐ์ `0 1 auto`์ด๋ค.

W3C์์๋ ์ด ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ๋ ๊ฒ ๋ณด๋ค ๊ฐ๋ณ์ ์ผ๋ก ๊ธฐ์ ํ๋ ๊ฒ์ ์ถ์ฒํ๊ณ  ์๋ค.

---

### 5-6. align-self

`align-item` ์์ฑ์ ๊ต์ฐจ์ถ์ ๊ธฐ์ค์ผ๋ก ํ๋ ์ค ํญ๋ชฉ์ ์ ๋ ฌ ๋ฐฉ๋ฒ์ ๊ฒฐ์ ํ์ง๋ง ๊ทธ์ค์์ ํน์  ํญ๋ชฉ๋ง ์ง์ ํ๊ณ  ์ถ๋ค๋ฉด `align-self` ์์ฑ์ ์ฌ์ฉํ๋ค. `align-item` ํ๋กํผํฐ๋ณด๋ค ์ฐ์ ํ์ฌ ๊ฐ๋ณ flex item์ ์ ๋ ฌํ๋ค.

`align-self` ํ๋กํผํฐ์์ ์ฌ์ฉํ ๊ฐ์ `align-items` ํ๋กํผํฐ์์ ์ฌ์ฉํ๋ ๊ฐ๊ณผ ๊ฐ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .flex-container {
        display: flex;
        justify-content: space-around;
        height: 600px;
        background-color: aliceblue;
      }
      .box {
        width: 100px;
        padding: 60px;
        background-color: yellow;
      }
      .box:not(:last-child) {
        margin-right: 10px;
      }
      .box:nth-child(1) {
        align-self: flex-start;
      }
      .box:nth-child(2) {
        align-self: flex-end;
      }
      .box:nth-child(3) {
        align-self: center;
      }
      .box:nth-child(4) {
        align-self: stretch;
      }
    </style>
  </head>
  <body>
    <div class="flex-container">
      <div class="box">1 / align-self: flex-start</div>
      <div class="box">2 / align-self: flex-end</div>
      <div class="box">3 / align-self: center</div>
      <div class="box">4 / align-self: stretch</div>
    </div>
  </body>
</html>
```

![align-self](../image/CSS/Flex/alignSelf.png)

---

## 6. Conclusion

> `flex-grow`๋ ์ฒ์ ์จ๋ณด๋ ํ๋กํผํฐ์ด๋ค. ๊ต์ฅํ ์ ๊ธฐํ๋ค. ์์ ์ฝ๋๋ฅผ ์๋ฅผ๋ค์ด box ํด๋์ค๋ฅผ ๊ฐ์ง div๋ค์ ๋๋น๋ฅผ 100%๋ก ์ง์ ํ์ง ์์๋ `flex-grow`๋ฅผ 1๋ก ์ง์ ํ๋ฉด ์ ์ฒด ๋๋น๋ฅผ ๋ชจ๋ ๋๋ฑํ๊ฒ ์ฐจ์งํ๊ฒ ๋๋ค. ๊ธฐ๋ณธ๊ฐ์ 0 ์ด๋ฏ๋ก ๋ฑ ์์ ์ ์์๋งํผ๋ง ๋๋น๋ฅผ ์ฐจ์งํ๋ ๊ฒ ๊ฐ๋ค.  
> ๋ฟ๋ง ์๋๋ผ `flex-shrink`๊ณผ `flex-basis`๋ ์ด๋ฒ ๊ณต๋ถ๋ฅผ ํ๋ฉด์ ์ฒ์ ์จ๋ณด๋ ํ๋กํผํฐ์๋ค.  
> `flex`๋ `grid`๋ฅผ ๋ฐฐ์ฐ๊ธฐ ์ ๊น์ง ์์ฃผ ์ฌ์ฉํ์๋ค. ํ์ง๋ง `grid`๋ฅผ ๋ฐฐ์ฐ๊ณ  ๋์๋ ์ฌ์ฉ๋น๋๊ฐ ๋ง์ด ์ค์๋ค. ๋ ๊ฐ์ ํ๋กํผํฐ ๋ชจ๋ CSS์์ ์ค์ํ ๋ด์ฉ์ด๋ ํ์ํ ์ํฉ์ ๋ง์ถฐ ์ ์ฌ์ฉํ๋๋ก ํ์.

---

## ์ฐธ๊ณ 

[poiemaweb 2-20 ํ๋ ์ค ๋ฐ์ค ๋ ์ด์์](https://poiemaweb.com/css3-flexbox)  
๋์ - HTML + CSS + ์๋ฐ์คํฌ๋ฆฝํธ ์น ํ์ค์ ์ ์

---

[๐](#flexbox-layout)
