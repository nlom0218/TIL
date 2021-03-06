# Text

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. line-height ํ๋กํผํฐ](#2-line-height-ํ๋กํผํฐ)
- [3. letter-spacing ํ๋กํผํฐ](#3-letter-spacing-ํ๋กํผํฐ)
- [4. text-align ํ๋กํผํฐ](#4-text-align-ํ๋กํผํฐ)
- [5. text-decoration ํ๋กํผํฐ](#5-text-decoration-ํ๋กํผํฐ)
- [6. white-space ํ๋กํผํฐ](#6-white-space-ํ๋กํผํฐ)
- [7. text-overflow ํ๋กํผํฐ](#7-text-overflow-ํ๋กํผํฐ)
- [8. word-wrap ํ๋กํผํฐ](#8-word-wrap-ํ๋กํผํฐ)
- [9. word-break ํ๋กํผํฐ](#9-word-break-ํ๋กํผํฐ)
- [10. text-transform ํ๋กํผํฐ](#10-text-transform-ํ๋กํผํฐ)
- [11. Conclusion](#11-conclusion)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

ํ์คํธ ์คํ์ผ์ ์ฌ์ฉํ๋ ๊ธ์์ ๋ชจ์์๋ฅผ ์ง์ ํ๋ ๊ธ๊ผด ์คํ์ผ๊ณผ ์น ๋ฌธ์์ ํ์๋๋ ํ์คํธ๋ฅผ ์ง์ ํ๋ ๋ฌธ๋จ ์คํ์ผ์ด ์๋ค.

---

## 2. line-height ํ๋กํผํฐ

ํ์คํธ์ ๋์ด๋ฅผ ์ง์ ํ๋ค. ํ ๋ฌธ๋จ์ด ๋ ์ค์ด ๋์ผ๋ฉด ์ค ๊ฐ๊ฒฉ์ด ์๊ธด๋ค. ์ค ๊ฐ๊ฒฉ์ด ๋๋ฌด ์ข๊ฑฐ๋ ๋์ผ๋ฉด ๊ฐ๋์ฑ์ด ๋จ์ด์ง๋ค. ์ด๋ `line-height` ์์ฑ์ ์ด์ฉํ๋ฉด ์ค ๊ฐ๊ฒฉ์ ์ํ๋ ๋งํผ ์กฐ์ ํ  ์ ์๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .small {
        line-height: 70%;
      }
      .large {
        line-height: 1.5;
      }
      .x-large {
        line-height: 3;
      }
    </style>
  </head>
  <body>
    <p>
      ๊ธฐ๋ณธ ์ค ๊ฐ๊ฒฉ<br />
      ๊ธฐ๋ณธ ์ค ๊ฐ๊ฒฉ<br />
    </p>
    <p class="small">
      ์ข์ ์ค ๊ฐ๊ฒฉ<br />
      ์ข์ ์ค ๊ฐ๊ฒฉ<br />
      line-height: 70%
    </p>
    <p class="large">
      ํฐ ์ค ๊ฐ๊ฒฉ<br />
      ํฐ ์ค ๊ฐ๊ฒฉ<br />
      line-height: 1.5
    </p>
    <p class="x-large">
      ๋งค์ฐ ํฐ ์ค ๊ฐ๊ฒฉ<br />
      ๋งค์ฐ ํฐ ์ค ๊ฐ๊ฒฉ<br />
      line-height: 3
    </p>
  </body>
</html>
```

![line-height](../image/CSS/Text/lineHeight.png)

๋ค์์ ์์ง ์ค์ ์ ๋ ฌ ์์ ์ด๋ค. a์์์ `line-height` ๊ฐ๊ณผ a์์๋ฅผ ๊ฐ์ธ๋ `div` ์์์ `height` ๊ฐ์ ์ผ์น์ํจ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .btn {
        width: 80px;
        height: 40px;
        background-color: orchid;
        border-radius: 40px;
      }
      .btn > a {
        display: block;
        color: aliceblue;
        text-align: center;
        text-decoration: none;
        line-height: 40px;
      }
    </style>
  </head>
  <body>
    <div class="btn">
      <a class="btn" href="#">Click</a>
    </div>
  </body>
</html>
```

![line-height2](../image/CSS/Text/lineHeight2.png)

---

## 3. letter-spacing ํ๋กํผํฐ

๊ธ์ ์ฌ์ด์ ๊ฐ๊ฒฉ์ ์ง์ ํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .ex1 {
        letter-spacing: -2px;
      }
      .ex2 {
        letter-spacing: 2px;
      }
      .ex3 {
        letter-spacing: 10px;
      }
    </style>
  </head>
  <body>
    <p>default letter spacing</p>
    <p class="ex1">letter-spacing: -2px</p>
    <p class="ex2">letter-spacing: 2px</p>
    <p class="ex3">letter-spacing: 10px</p>
  </body>
</html>
```

![letter-spacing](../image/CSS/Text/letterSpacing.png)

---

## 4. text-align ํ๋กํผํฐ

ํ์คํธ์ ์ํ ์ ๋ ฌ์ ์ ์ํ๋ค. ํ๊ธ ๋ฌธ์์์ ํํ ์ฌ์ฉํ๋ ์ผ์ชฝ ์ ๋ ฌ, ์ค๋ฅธ์ชฝ ์ ๋ ฌ, ์์ชฝ ์ ๋ ฌ, ๊ฐ์ด๋ฐ ์ ๋ ฌ ๋ฑ์ ์น ๋ฌธ์์์๋ ์ง์ ํ  ์ ์๋ค.

์๋์ ํ๋ `text-align`์ ํ๋กํผํฐ๊ฐ์ด๋ค.
|์ข๋ฅ|์ค๋ช|
|:---|:---|
|start|ํ์ฌ ํ์คํธ ์ค์ ์์ ์์น์ ๋ง์ถ์ด ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|
|end|ํ์ฌ ํ์คํธ ์ค์ ๋ ์์น์ ๋ง์ถ์ด ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|
|left|์ผ์ชฝ์ ๋ง์ถ์ด ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|
|right|์ค๋ฅธ์ชฝ์ ๋ง์ถ์ด ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|
|center|๊ฐ์ด๋ฐ์ ๋ง์ถ์ด ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|
|justify|์์ชฝ์ ๋ง์ถ์ด ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|
|match-parent|๋ถ๋ชจ ์์๋ฅผ ๋ฐ๋ผ ๋ฌธ๋จ์ ์ ๋ ฌํ๋ค.|

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      p {
        background-color: antiquewhite;
      }
      .start {
        text-align: start;
      }
      .end {
        text-align: end;
      }
      .left {
        text-align: left;
      }
      .right {
        text-align: right;
      }
      .center {
        text-align: center;
      }
      .justify {
        text-align: justify;
      }
    </style>
  </head>
  <body>
    <p class="start">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio esse in
      amet quos aliquam temporibus dicta adipisci, magnam, corrupti velit quis
      fugiat voluptatum provident possimus assumenda quaerat totam delectus
      tempore.
    </p>
    <p class="end">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio esse in
      amet quos aliquam temporibus dicta adipisci, magnam, corrupti velit quis
      fugiat voluptatum provident possimus assumenda quaerat totam delectus
      tempore.
    </p>
    <p class="left">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio esse in
      amet quos aliquam temporibus dicta adipisci, magnam, corrupti velit quis
      fugiat voluptatum provident possimus assumenda quaerat totam delectus
      tempore.
    </p>
    <p class="right">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio esse in
      amet quos aliquam temporibus dicta adipisci, magnam, corrupti velit quis
      fugiat voluptatum provident possimus assumenda quaerat totam delectus
      tempore.
    </p>
    <p class="center">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio esse in
      amet quos aliquam temporibus dicta adipisci, magnam, corrupti velit quis
      fugiat voluptatum provident possimus assumenda quaerat totam delectus
      tempore.
    </p>
    <p class="justify">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio esse in
      amet quos aliquam temporibus dicta adipisci, magnam, corrupti velit quis
      fugiat voluptatum provident possimus assumenda quaerat totam delectus
      tempore.
    </p>
  </body>
</html>
```

![text-align](../image/CSS/Text/textAlign.png)

---

## 5. text-decoration ํ๋กํผํฐ

`text-decoration` ํ๋กํผํฐ๋ ํ์คํธ์ ๋ฐ์ค์ ๊ธ๊ฑฐ๋ ์ทจ์์ ์ ํ์ํ๋ค. ๋งํฌ์ underline๋ฅผ ์ ๊ฑฐํ  ์ ์์ผ๋ฉฐ ํ์คํธ์ underline, overline, line-through๋ฅผ ์ถ๊ฐํ  ์๋ ์๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      a {
        text-decoration: none;
      }
      p:nth-of-type(1) {
        text-decoration: overline;
      }
      p:nth-of-type(2) {
        text-decoration: line-through;
      }
      p:last-child {
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <a href="#">text-decoratoin: none</a>
    <p>text-decoration: overline</p>
    <p>text-decoration: line-through</p>
    <p>text-decoration: underline</p>
  </body>
</html>
```

![text-decoration](../image/CSS/Text/textDecoration.png)

---

## 6. white-space ํ๋กํผํฐ

white space๋ ๊ณต๋ฐฑ(space), ๋ค์ฌ์ฐ๊ธฐ(tab), ์ค๋ฐ๊ฟ(line break)์ ์๋ฏธํ๋ค. html์ ๊ธฐ๋ณธ์ ์ผ๋ก ์ฐ์๋ ๊ณต๋ฐฑ, ๋ค์ฌ์ฐ๊ธฐ๋ 1๋ฒ๋ง ์คํ๋๋ฉฐ ์ค๋ฐ๊ฟ์ ๋ฌด์๋๋ค. ๋ํ ํ์คํธ๋ ๋ถ๋ชจ์ ๊ฐ๋ก ์์ญ์ ๋ฒ์ด๋์ง ์๊ณ  ์๋ ์ค๋ฐ๊ฟ(wrap)๋๋ค. `white-space` ํ๋กํผํฐ๋ ์ด๋ฌํ ๊ธฐ๋ณธ ๋์์ ์ ์ดํ๊ธฐ ์ํ ํ๋กํผํฐ๋ค.

| ํ๋กํผํฐ | line break | space/tab   | wrapping(์๋์ค๋ฐ๊ฟ) |
| :------- | :--------- | :---------- | -------------------- |
| normal   | ๋ฌด์       | 1๋ฒ๋ง ๋ฐ์  | O                    |
| nowrap   | ๋ฌด์       | 1๋ฒ๋ง ๋ฐ์  | X                    |
| pre      | ๋ฐ์       | ๊ทธ๋๋ก ๋ฐ์ | X                    |
| pre-wrap | ๋ฐ์       | ๊ทธ๋๋ก ๋ฐ์ | O                    |
| pre-line | ๋ฐ์       | 1๋ฒ๋ง ๋ฐ์  | O                    |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        display: grid;
        grid-template-columns: 1fr 1fr;
      }
      div {
        width: 150px;
        height: 150px;
        border: 1px solid blueviolet;
        margin-bottom: 120px;
      }
      h3 {
        margin: 0;
      }
      .nomal {
        white-space: normal;
      }
      .nowrap {
        white-space: nowrap;
      }
      .pre {
        white-space: pre;
      }
      .pre-wrap {
        white-space: pre-wrap;
      }
      .pre-line {
        white-space: pre-line;
      }
    </style>
  </head>
  <body>
    <div class="nomal">
      <h3>normal</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore quo hic
      aspernatur deserunt veritatis nemo, facilis totam autem laborum tempora
      distinctio quae cum ut maxime obcaecati eligendi porro vitae praesentium.
    </div>
    <div class="nowrap">
      <h3>nowrap</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore quo hic
      aspernatur deserunt veritatis nemo, facilis totam autem laborum tempora
      distinctio quae cum ut maxime obcaecati eligendi porro vitae praesentium.
    </div>
    <div class="pre">
      <h3>pre</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore quo hic
      aspernatur deserunt veritatis nemo, facilis totam autem laborum tempora
      distinctio quae cum ut maxime obcaecati eligendi porro vitae praesentium.
    </div>
    <div class="pre-wrap">
      <h3>pre-wrap</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore quo hic
      aspernatur deserunt veritatis nemo, facilis totam autem laborum tempora
      distinctio quae cum ut maxime obcaecati eligendi porro vitae praesentium.
    </div>
    <div class="pre-line">
      <h3>pre-line</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore quo hic
      aspernatur deserunt veritatis nemo, facilis totam autem laborum tempora
      distinctio quae cum ut maxime obcaecati eligendi porro vitae praesentium.
    </div>
  </body>
</html>
```

![white-space](../image/CSS/Text/whiteSpace.png)

---

## 7. text-overflow ํ๋กํผํฐ

๋ถ๋ชจ ์์ฌ๊ธใน ๋ฒ์ด๋ wrapping(์๋์ค๋ฐ๊ฟ)์ด ๋์ง ์์ ํ์คํธ์ ์ฒ๋ฆฌ ๋ฐฉ๋ฒ์ ์ ์ํ๋ค.

- ์กฐ๊ฑด
  - `width` ํ๋กํผํฐ๊ฐ ์ง์ ๋์ด ์์ด์ผ ํ๋ค. ์ด๋ฅผ ์ํด ํ์ํ ๊ฒฝ์ฐ block๋ ๋ฒจ ์์๋ก ๋ณ๊ฒฝํด์ผ ํ๋ค.
  - ์๋ ์ค๋ฐ๊ฟ์ ๋ฐฉ์งํ๋ ค๋ฉด `white-space` ํ๋กํผํฐ๋ฅผ nowrap์ผ๋ก ์ค์ ํ๋ค.
  - overflow ํ๋กํผํฐ์ ๋ฐ๋์ `visible` ์ด์ธ์ ๊ฐ์ด ์ง์ ๋์ด ์์ด์ผ ํ๋ค.

์๋๋ `text-overflow` ํ๋กํผํฐ์ ์ค์ ํ  ์ ์๋ ํ๋กํผํฐ ๊ฐ์ด๋ค.
|ํ๋กํผํฐ ๊ฐ|์ค๋ช|
|:---|:---|
|clip|์์ญ์ ๋ฒ์ด๋ ํ์คํธ๋ฅผ ํ์ํ์ง ์๋๋ค.(๊ธฐ๋ณธ๊ฐ)|
|ellipsis|์์ญ์ ๋ฒ์ด๋ ํ์คํธ๋ฅผ ์๋ผ๋ด์ด ๋ณด์ด์ง ์๊ฒ ํ๊ณ  ๋ง์ค์ํ(...)๋ฅผ ํ์ํ๋ค.|

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        width: 150px;
        height: 150px;
        border: 1px solid blueviolet;
        white-space: nowrap;
        overflow: hidden;
        display: inline-block;
      }
      h3 {
        margin: 0;
      }
      .clip {
        text-overflow: clip;
      }
      .ellipsis {
        text-overflow: ellipsis;
      }
    </style>
  </head>
  <body>
    <div class="clip">
      <h3>clip</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit.
    </div>
    <div class="ellipsis">
      <h3>ellipsis</h3>
      Lorem ipsum dolor sit amet consectetur adipisicing elit.
    </div>
  </body>
</html>
```

![text-overflow](../image/CSS/Text/textOverflow.png)

---

## 8. word-wrap ํ๋กํผํฐ

ํ ๋จ์ด์ ๊ธธ์ด๊ฐ ๊ธธ์ด์ ๋ถ๋ชจ ์์ญ์ ๋ฒ์ด๋ ํ์คํธ์ ์ฒ๋ฆฌ ๋ฐฉ๋ฒ์ ์ ์ํ๋ค.

์ฌ์ฉ๋ฒ `word-wrap: break-word;`

---

## 9. word-break ํ๋กํผํฐ

`word-wrap` ํ๋กํผํฐ์ ๊ฐ์ด ํ ๋จ์ด์ ๊ธธ์ด๊ฐ ๊ธธ์ด์ ๋ถ๋ชจ ์์ญ์ ๋ฒ์ด๋ ํ์คํธ์ ์ฒ๋ฆฌ ๋ฐฉ๋ฒ์ ์ ์ํ๋ค. `word-wrap` ํ๋กํผํฐ ๊ฒฝ์ฐ ๋จ์ด๋ฅผ ์ด๋ ์ ๋๋ ๊ณ ๋ คํ์ฌ ๊ฐํํ์ง๋ง `word-break: break-all;`๋ ๋จ์ด๋ฅผ ๊ณ ๋ คํ์ง ์๊ณ  ๋ถ๋ชจ ์์ญ์ ๋ง์ถ์ด ๊ฐ์  ๊ฐํํ๋ค.

์ฌ์ฉ๋ฒ `word-break: break-all;`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        display: flex;
        justify-content: space-around;
      }
      div {
        width: 150px;
        height: 300px;
        border: 1px solid black;
        margin-bottom: 10px;
      }
      div:nth-of-type(2) {
        word-wrap: break-word;
      }
      div:nth-of-type(3) {
        word-break: break-all;
      }
    </style>
  </head>
  <body>
    <div>abcdefghijklmnopqrstuvwxyz https://www.teachercan.com/</div>
    <div>
      word-wrap: break-word; abcdefghijklmnopqrstuvwxyz
      https://www.teachercan.com/
    </div>
    <div>
      word-break: break-all; abcdefghijklmnopqrstuvwxyz
      https://www.teachercan.com/
    </div>
  </body>
</html>
```

![word-break](../image/CSS/Text/wordBreak.png)

---

## 10. text-transform ํ๋กํผํฐ

์๋ฌธ์๋ฅผ ํ๊ธฐํ  ๋ ํ์คํธ์ ๋์ ๋ฌธ์๋ฅผ ์ํ๋ ๋๋ก ๋ฐ๊ฟ ๋ `text-transform` ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ๋ค. ํ๊ธ์๋ ์ํฅ์ ๋ฏธ์น์ง ์๊ณ  ์๋ฌธ์์๋ง ์ ์ฉ๋๋ค.

์๋๋ `text-transform` ํ๋กํผํฐ๊ฐ์ด๋ค.
|์ข๋ฅ|์ค๋ช|
|:---|:---|
|capitalize|๊ฐ ๋จ์ด์ ์ฒซ ๋ฒ์งธ ๊ธ์๋ฅผ ๋๋ฌธ์๋ก ๋ณํํ๋ค.|
|uppercase|๋ชจ๋  ๊ธ์๋ฅผ ๋๋ฌธ์๋ก ๋ณํํ๋ค.|
|lowercase|๋ชจ๋  ๊ธ์๋ฅผ ์๋ฌธ์๋ก ๋ณํํ๋ค.|

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        display: flex;
        justify-content: space-around;
      }
      .ex1 {
        text-transform: capitalize;
      }
      .ex2 {
        text-transform: uppercase;
      }
      .ex3 {
        text-transform: lowercase;
      }
    </style>
  </head>
  <body>
    <span class="ex1">hello world</span>
    <span class="ex2">hello world</span>
    <span class="ex3">HELLOW WORLD</span>
  </body>
</html>
```

![text-transform](../image/CSS/Text/textTransform.png)

---

## 11. Conclusion

> text๊ด๋ จ ํ๋กํผํฐ๋ ๋ง์ด ์ฌ์ฉ์ ํ์ง๋ง `white-space`, `text-overflow`, `word-wrap`, `word-break` ํ๋กํผํฐ๋ ํฐ์ฒ์บ์ ๋ง๋ค ๋๋ง ์ฌ์ฉํ์ฌ ์ต์ํ์ง ์๋ ํ๋กํผํฐ์ด๋ค. ๊ทธ๋งํผ ๋ค๋ฅธ ํ๋กํผํฐ์ ๋นํด ๋น์ค์ด ๋จ์ด์ง๋ ๊ฑฐ ๊ฐ๋ค. ํ์ง๋ง ์น ํ์ด์ง๋ฅผ ๋ง๋ค๋ฉด์ ํ์ํ ์ํฉ์ด ์ฌ ์ ์๊ธฐ์ ํด๋น ํ๋กํผํฐ์ ํน์ง์ ์ ์์๋๊ณ  ์์.

---

## ์ฐธ๊ณ 

[poiemaweb 2-7 ํฐํธ์ ํ์คํธ](https://poiemaweb.com/css3-font-text)  
๋์ - HTML + CSS + ์๋ฐ์คํฌ๋ฆฝํธ ์น ํ์ค์ ์ ์

---

[๐](#text)
