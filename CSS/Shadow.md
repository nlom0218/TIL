# Shadow

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. text-shadow](#2-text-shadow)
- [3. box-shadow](#3-box-shadow)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

`shadow`๋ ํ์คํธ๋ ์์์ ๊ทธ๋ฆผ์ํจ๊ณผ๋ฅผ ๋ถ์ฌํ๊ธฐ ์ํ ํ๋กํผํฐ์ด๋ค.

## 2. text-shadow

`text-shadow`๋ ๊ธ์์ ๊ทธ๋ฆผ์ ํจ๊ณผ๋ฅผ ์ฃผ๋ ํ๋กํผํฐ์ด๋ค.

์ฌ์ฉ๋ฒ `text-shadow: offset-x offset-y blur-radius color | none | initial | inherit`

์๋๋ `text-shadow`์ ํ๋กํผํฐ ๊ฐ์ ์ค๋ช์ด๋ค.

| ํ๋กํผํฐ ๊ฐ  | ์ค๋ช                                                               | ์๋ต   |
| :----------- | :----------------------------------------------------------------- | :----- |
| offset-x     | ๊ทธ๋ฆผ์๋ฅผ ํ์คํธ์ ์ค๋ฅธ์ชฝ์ผ๋ก ์ง์ ๊ฐ๋งํผ ์ด๋์ํจ๋ค.                | ๋ถ๊ฐ๋ฅ |
| offset-y     | ๊ทธ๋ฆผ์๋ฅผ ํ์คํธ์ ์๋๋ก ์ง์ ๊ฐ๋งํผ ์ด๋์ํจ๋ค.                    | ๋ถ๊ฐ๋ฅ |
| blur-radius  | ๊ทธ๋ฆผ์์ ํ๋ฆผ์ ๋๋ฅผ ์ง์ ํ๋ค. ์ง์ ๊ฐ๋งํผ ๊ทธ๋ฆผ์๊ฐ ์ปค์ง๊ณ  ํ๋ ค์ง๋ค. | ๊ฐ๋ฅ   |
| shadow-color | ๊ทธ๋ฆผ์์ ์์์ ์ง์ ํ๋ค.                                          | ๊ฐ๋ฅ   |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      h2:nth-child(1) {
        text-shadow: 3px 3px;
      }
      h2:nth-child(2) {
        text-shadow: 3px 3px 3px;
      }
      h2:nth-child(3) {
        text-shadow: 3px 3px 3px gray;
      }
      h2:nth-child(4) {
        color: white;
        text-shadow: 3px 3px 3px black;
      }
      h2:nth-child(5) {
        color: white;
        text-shadow: 0px 0px 3px black;
      }
      h2:nth-child(6) {
        color: white;
        text-shadow: 3px 3px 3px black, 3px 3px 3px gray;
      }
    </style>
  </head>
  <body>
    <h2>Hello World</h2>
    <h2>Hello World</h2>
    <h2>Hello World</h2>
    <h2>Hello World</h2>
    <h2>Hello World</h2>
    <h2>Hello World</h2>
  </body>
</html>
```

![text-shadow](../image/CSS/Shadow/textShadow.png)

---

## 3. box-shadow

`box-shadow`๋ ์ ํํ ์์์ ๊ทธ๋์ ํจ๊ณผ๋ฅผ ๋ง๋ค์ด ์ค๋ค.

์ฌ์ฉ๋ฒ `box-shadow: x-position y-position blur spread color | inset | initial | inherit`

์๋๋ `box-shadow`์ ํ๋กํผํฐ ๊ฐ์ ์ค๋ช์ด๋ค.

| ํ๋กํผํฐ ๊ฐ | ์ค๋ช                                                         | ์๋ต   |
| :---------- | :----------------------------------------------------------- | :----- |
| x-position  | ๊ฐ๋ก ์์น, ์์๋ฉด ์ค๋ฅธ์ชฝ, ์์๋ฉด ์ผ์ชฝ์ ๊ทธ๋ฆผ์๊ฐ ๋ง๋ค์ด์ง๋ค. | ๋ถ๊ฐ๋ฅ |
| y-position  | ์ธ๋ก ์์น, ์์๋ฉด ์๋์ชฝ, ์์๋ฉด ์์ชฝ์ ๊ทธ๋ฆผ์๊ฐ ๋ง๋ค์ด์ง๋ค. | ๋ถ๊ฐ๋ฅ |
| blur        | ๊ทธ๋ฆผ์๋ฅผ ํ๋ฆฟํ๊ฒ ๋ง๋ ๋ค. ๊ฐ์ด ํด์๋ก ๋์ฑ ํ๋ ค์ง๋ค.         | ๊ฐ๋ฅ   |
| spread      | ์์๋ฉด ๊ทธ๋ฆผ์๋ฅผ ํ์ฅํ๊ณ , ์์๋ฉด ์ถ์ํ๋ค.                   | ๊ฐ๋ฅ   |
| color       | ๊ทธ๋ฆผ์ ์์ ์ ํ๋ค.                                          | ๊ฐ๋ฅ   |
| inset       | ๊ทธ๋ฆผ์๋ฅผ ์์์ ์์ชฝ์ ๋ง๋ ๋ค.                               | ๊ฐ๋ฅ   |
| inital      | ๊ธฐ๋ณธ๊ฐ์ผ๋ก ์ค์ ํ๋ค.                                         | ๊ฐ๋ฅ   |
| inherit     | ๋ถ๋ชจ ์์์ ์์ฑ๊ฐ์ ์์๋ฐ๋๋ค.                             | ๊ฐ๋ฅ   |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        display: inline-block;
        width: 120px;
        height: 120px;
        background-color: green;
        margin-right: 20px;
      }
      div:nth-child(1) {
        box-shadow: 10px 10px;
      }
      div:nth-child(2) {
        box-shadow: 10px 10px gray;
      }
      div:nth-child(3) {
        box-shadow: 10px 10px 10px gray;
      }
      div:nth-child(4) {
        box-shadow: 10px 10px 10px 5px gray;
      }
      div:nth-child(5) {
        box-shadow: 10px 10px 10px -5px gray;
      }
      div:nth-child(6) {
        box-shadow: 10px 10px 10px -5px gray inset;
      }
    </style>
  </head>
  <body>
    <div>Hello World</div>
    <div>Hello World</div>
    <div>Hello World</div>
    <div>Hello World</div>
    <div>Hello World</div>
    <div>Hello World</div>
  </body>
</html>
```

![box-shadow](../image/CSS/Shadow/boxShadow.png)

---

## ์ฐธ๊ณ 

[CSS / text-shadow / ๊ธ์์ ๊ทธ๋ฆผ์ ํจ๊ณผ๋ฅผ ์ฃผ๋ ์์ฑ](https://www.codingfactory.net/10650)  
[CSS / box-shadow / ๋ฐ์ค์ ๊ทธ๋ฆผ์ ํจ๊ณผ ๋ง๋๋ ์์ฑ](https://www.codingfactory.net/10628)  
[poiemaweb 2-12 ๊ทธ๋ฆผ์](https://poiemaweb.com/css3-shadow)

---

[๐](#shadow)
