# display νλ‘νΌν° (block, inline, inline-block)

## π λ°λ‘κ°κΈ°

- [1. κ°μ](#1-κ°μ)
- [2. Block λ λ²¨ μμ](#2-block-λ λ²¨-μμ)
- [3. inline λ λ²¨ μμ](#3-inline-λ λ²¨-μμ)
- [4. inline-block λ λ²¨ μμ](#4-inline-block-λ λ²¨-μμ)
- [5. Conclusion](#5-conclusion)
- [μ°Έκ³ ](#μ°Έκ³ )

---

## 1. κ°μ

display νλ‘νΌν°λ layout μ μμ μμ£Ό μ¬μ©λλ μ€μν νλ‘νΌν°  
[λ°μ€ λͺ¨λΈ](./BoxModel.md)μ block λ λ²¨ μμμΈμ§ inline λ λ²¨ μμμΈμ§μ λ°λΌ λμ΄ λ°©λ²μ΄ λ¬λΌμ§

| νλ‘νΌν°κ° ν€μλ | μ€λͺ                                                           |
| :---------------- | :------------------------------------------------------------- |
| block             | block νΉμ±μ κ°μ§λ μμ(block λ λ²¨ μμ)λ‘ μ§μ                |
| inline            | inline νΉμ±μ κ°μ§λ μμ(inline λ λ²¨ μμ)λ‘ μ§μ              |
| inline-block      | inline-block νΉμ±μ κ°μ§λ μμ(inline-block λ λ²¨ μμ)λ‘ μ§μ  |
| none              | ν΄λΉ μμλ₯Ό νλ©΄μ νμνμ§ μμ(κ³΅κ°μ‘°μ°¨ μ¬λΌμ§)              |

> display νλ‘νΌν°λ μμλμ§ μλλ€.

---

## 2. Block λ λ²¨ μμ

- ν­μ μλ‘μ΄ λΌμΈμμ μμνλ€.
- νλ©΄ ν¬κΈ° μ μ²΄μ κ°λ‘ν­μ μ°¨μ§νλ€.(width:100%)
- width, height, margin, padding νλ‘νΌν° μ§μ μ΄ κ°λ₯νλ€.
- block λ λ²¨ μμ λ΄μ inline λ λ²¨ μμλ₯Ό ν¬ν¨ν  μ μλ€.
- block λ λ²¨ μμ μ
  - div
  - h1 ~ h6
  - p
  - ol
  - ul
  - li
  - hr
  - table
  - form

```html
<!DOCTYPE html>
<head>
  <style>
    body {
      padding: 10px;
      color: white;
    }
    div {
      background-color: red;
      padding: 10px;
      border: 5px solid pink;
    }
    h1 {
      background-color: blue;
      width: 320px;
      margin: 10px 0px;
      font-size: 16px;
    }
    p {
      background-color: green;
      width: 120px;
      height: 120px;
    }
  </style>
</head>
<body>
  <div>λΈλ‘ λ λ²¨ μμ div</div>
  <h1>λΈλ‘ λ λ²¨ μμ h1</h1>
  <p>λΈλ‘ λ λ²¨ μμ p</p>
</body>
```

![block level](../image/CSS/BlockLevel.png)

---

## 3. inline λ λ²¨ μμ

- μλ‘μ΄ λΌμΈμμ μμνμ§ μμΌλ©° λ¬Έμ₯μ μ€κ°μ λ€μ΄κ° μ μλ€. μ¦, μ€μ λ°κΎΈμ§ μκ³  λ€λ₯Έ μμμ ν¨κ» ν νμ μμΉνλ€.
- contentμ λλΉλ§νΌ κ°λ‘ν­μ μ°¨μ§νλ€.
- **width ,height, margin-top, margin-bottom νλ‘νΌν°λ₯Ό μ§μ ν  μ μλ€.** μ, ν μ¬λ°±μ line-heightλ‘ μ§μ νλ€.
- inline λ λ²¨ μμ λ€μ κ³΅λ°±(μν°, μ€νμ΄μ€ λ±)μ΄ μλ κ²½μ°, μ μνμ§ μμ space(4px)κ° μλ μ§μ λλ€.
- inline λ λ²¨ μμ λ΄μ block λ λ²¨ μμλ₯Ό ν¬ν¨ν  μ μλ€. inline λ λ²¨ μμλ μΌλ°μ μΌλ‘ block λ λ²¨ μμμ ν¬ν¨λμ΄ μ¬μ©λλ€.
- inline λ λ²¨ μμ μ

  - span
  - a
  - strong
  - img
  - br
  - input
  - select
  - textarea
  - button

  ```html
  <!DOCTYPE html>
  <head>
    <style>
      span {
        /* margin-top, margin-bottomμ μ μ©λμ§ μλλ€. */
        /* νμ§λ§ margin-right, margin-leftμ μ μ©λλ€.*/
        margin: 200px 100px;
        background-color: pink;
        /* paddingμ κ°λ₯νλ€. */
        /* paddingμ΄ ν΄ κ²½μ° λ€λ₯Έ boxλ₯Ό μΉ¨λ²νλ κ²½μ°λ μκΈ΄λ€...? */
        padding: 10px;
      }
    </style>
  </head>
  <body>
    <h1>
      μ΄κ±΄ block λ λ²¨ μμμ λνμ μΈ h1μλλ€.
      <span>κ·Έ μμ spanμ΄ μλ€μ^^</span>
    </h1>
    <span>span1</span>
    <span>span2</span><span>span3</span>
  </body>
  ```

![inline level](../image/CSS/InlineLevel.png)

---

## 4. inline-block λ λ²¨ μμ

blockκ³Ό inline λ λ²¨ μμμ νΉμ§μ λͺ¨λ κ°λλ€. inline λ λ²¨ μμμ κ°μ΄ ν μ€μ ννλλ©΄μ width, height, margin νλ‘νΌν°λ₯Ό λͺ¨λ μ§μ ν  μ μλ€.

- κΈ°λ³Έμ μΌλ‘ inline λ λ²¨ μμμ ν‘μ¬νκ² μ€μ λ°κΎΈμ§ μκ³  λ€λ₯Έ μμμ ν¨κ» ν νμ μμΉμν¬ μ μλ€.
- block λ λ²¨ μμμ²λΌ width, height, margin, padding νλ‘νΌν°λ₯Ό λͺ¨λ μ μν  μ μλ€. μ, ν μ¬λ°±μ marginκ³Ό line-height λκ°μ§ νλ‘νΌν° λͺ¨λλ₯Ό ν΅ν΄ μ μ΄ν  μ μλ€.
- contentμ λλΉλ§νΌ κ°λ‘ν­μ μ°¨μ§νλ€.
- inline-block λ λ²¨ μμ λ€μ κ³΅λ°±(μν°, μ€νμ΄μ€ λ±)μ΄ μλ κ²½μ°, μ μνμ§ μμ space(4px)κ° μλ μ§μ λλ€.

```html
<!DOCTYPE html>
<head>
  <style>
    body {
      color: white;
    }
    .inline-block {
      display: inline-block;
      background-color: black;
      vertical-align: top;
    }
    .box1 {
      width: 120px;
      height: 120px;
    }
    .box2 {
      width: 120px;
      height: 160px;
    }
    .box3 {
      width: 240px;
      height: 120px;
    }
    .box4 {
      width: 240px;
      height: 160px;
    }
  </style>
</head>
<body>
  <span class="inline-block box1">inline-block width:120px height: 120px</span>
  <span class="inline-block box2">inline-block width:120px height: 160px</span>
  <div class="inline-block box3">inline-block width:240px height: 120px</div>
  <div class="inline-block box4">inline-block width:240px height: 160px</div>
</body>
```

![inline-block level](../image/CSS/InlineBlockLevel.png)

---

## 5. Conclusion

> μ§κΈκΉμ§ block, inline, inline-blockμ΄ μλ€λ μ¬μ€λ§ μκ³  μ νν μ°μμ λν΄μλ μκ°νμ§ μκ³  μ¬μ©μ νμλ€. μ΄λ‘ μΈν΄ κ²ͺμλ κ³ ν΅μ `input`μ κΈ°λ³ΈμΌλ‘ inlineμ΄μ¬μ widthκ° μ μ©λμ§ μμλ κ²μ΄λ€. block λ°κΎΈμλλΌλ©΄... μ½κ² ν΄κ²°μ ν  μ μμ§ μμμκΉ? κ·Έλ¦¬κ³  inline-blockμ νμ κΉμ§ μ¬λ¬ νλ‘μ νΈλ₯Ό μ§ννλ λμ μ¬μ©νμ§ μμλ€.  
> μ νν κ°λμ λͺ°λΌμ μ¬μ©μ νμ§ μμλ κ²μΈμ§? κ΅³μ΄ inline-blockμΌλ‘ μ¬μ©νμ§ μμλ flexλ gridμΌλ‘ ν΄κ²°ν  μ μλ λ¬Έμ  μλμ§λ λͺ¨λ₯΄κ² μ§λ§ νΉλ³ν κ²½μ°κ° μλλΌλ©΄ κ³μν΄μ flex λλ gridλ‘ νλ©΄ λμμΈμ ν  κ±° κ°λ€. νμ§λ§ νκ·Έλ€μ΄ κ°μ§κ³  μλ κΈ°λ³Έ display levelμ μκ³  νμν  λ λ°κΎΈμ΄ μ°λλ‘ νμ!

---

## μ°Έκ³ 

[poiemaweb 2-5 display, visibility, opacity νλ‘νΌν°](https://poiemaweb.com/css3-display)  
λμ - HTML + CSS + μλ°μ€ν¬λ¦½νΈ μΉ νμ€μ μ μ

---

[π](#display-νλ‘νΌν°-block-inline-inline-block)
