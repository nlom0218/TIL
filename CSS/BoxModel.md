# Box Model (λ°μ€ λͺ¨λΈ)

## π λ°λ‘κ°κΈ°

- [1. κ°μ](#1-κ°μ)
- [2. width / height νλ‘νΌν°](#2-width--height-νλ‘νΌν°)
- [3. margin / padding νλ‘νΌν°](#3-margin--padding-νλ‘νΌν°)
- [4. border νλ‘νΌν°](#4-border-νλ‘νΌν°)
  - [4-1. border-style](#4-1-border-style)
  - [4-2. border-width](#4-2-border-width)
  - [4-3. border-color](#4-3-border-color)
  - [4-4. border-radius](#4-4-border-radius)
  - [4-5. border](#4-5-border)
- [5. box-sizing νλ‘νΌν°](#5-box-sizing-νλ‘νΌν°)
- [6. Conclusion](#6-conclusion)
- [μ°Έκ³ ](#μ°Έκ³ )

---

## 1. κ°μ

μΉ λ¬Έμμ λΈλ‘ λ λ²¨ μμλ λͺ¨λ Box ννμ μμ­μ κ°μ§κ³  μμ. μ€νμΌ μνΈμμ μ΄λ κ² Box ννμΈ μμλ₯Ό λ°μ€ λͺ¨λΈ μμλΌκ³  ν¨. μ΄ Boxλ μ½νμΈ (Content), ν¨ν(Padding), νλλ¦¬(Border), λ§μ§(Margin)λ‘ κ΅¬μ±λ¨.

![Box Model](../image/CSS/BoxModel.png)

> Box Modelμ μ΄ν΄λ μλμ 4κ°μ§ λͺμΉ­μ μ΄ν΄νλλ°μ λΆν° μμνλ€.

| λͺμΉ­    | μ€λͺ                                                                                                                                                                                         |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Content | μμμ νμ€νΈλ μ΄λ―Έμ§ λ±μ μ€μ  λ΄μ©μ΄ μμΉνλ μμ­, width, height νλ‘νΌν°λ₯Ό κ°λλ€.                                                                                                      |
| Padding | νλλ¦¬(Border) μμͺ½μ μμΉνλ μμμ λ΄λΆ μμ­μ μ¬λ°±, padding νλ‘νΌν° κ°μ ν¨λ© μμ­μ λκΉ¨λ₯Ό μλ―Ένλ©° κΈ°λ³Έμμ ν¬λͺ(transparent), μμμ μ μ©λ λ°°κ²½μ μ»¬λ¬, μ΄λ―Έμ§λ ν¨λ© μμ­κΉμ§ μ μ© |
| Border  | νλλ¦¬ μμ­μΌλ‘ border νλ‘νΌν° κ°μ νλλ¦¬μ λκ»λ₯Ό μλ―Έ                                                                                                                                    |
| Margin  | νλλ¦¬(Border) λ°κΉ₯μ μμΉνλ μμμ μΈλΆ μ¬λ°± μμ­, margin νλ‘νΌν° κ°μ λ§μ§ μμ­μ λκ»λ₯Ό μλ―Έ, κΈ°λ³Έμ μΌλ‘ ν¬λͺ(transparent)νλ©° λ°°κ²½μμ μ§μ ν  μ μμ                                 |

```html
<!DOCTYPE html>
<head>
  <style>
    body {
      width: 50vw;
    }
    div {
      /* λ°°κ²½μμ μ§μ : μ½νμΈ  μμ­κ³Ό ν¨λ© μμ­μ μ μ© */
      background-color: red;

      /* ν¨λ© μμ­μ λκ» */
      padding: 20px;

      /* νλλ¦¬: λκ» νν μμ */
      border: 20px solid sandybrown;

      /* λ§μ§ μμ­μ λκ» */
      margin: 20px;
    }
  </style>
</head>
<body>
  <div>
    Lorem, ipsum dolor sit amet consectetur adipisicing elit. Recusandae
    doloribus, alias nemo ex aperiam itaque delectus dignissimos esse, quibusdam
    odio labore repellat vitae ullam obcaecati quod vero magni perspiciatis
    assumenda.
  </div>
</body>
```

![content, padding, border, margin](../image/CSS/BoxModel2.png)
![styled](../image/CSS/BoxModel3.png)

> μΉλμμΈμ μ½νμΈ λ₯Ό λ΄μ λ°μ€ λͺ¨λΈμ μ μνκ³  CSS νλ‘νΌν°λ₯Ό ν΅ν΄ μ€νμΌ(λ°°κ²½, ν°νΈμ νμ€νΈ λ±)κ³Ό μμΉ λ° μ λ ¬μ μ§μ νλ κ²

---

## 2. width / height νλ‘νΌν°

λ°μ€ λͺ¨λΈμμ μ½νμΈ  μμ­μ ν¬κΈ°λ₯Ό μ§μ ν  λ λλΉλ width, λμ΄λ height νλ‘νΌν°λ₯Ό μ¬μ©ν¨, μ½νμΈ μ μμ­μ λμμ νλ μ΄μ λ `box-sizing νλ‘νΌν°`μ κΈ°λ³Έκ°μΈ **content-box**κ° μ μ©λμκΈ° λλ¬Έ, `box-sizing νλ‘νΌν°`μ **border-box**λ₯Ό μ μ©νλ©΄ μ½νμΈ  μμ­, padding, borderκ° ν¬ν¨λ μμ­μ width / height νλ‘νΌν°μ λμμΌλ‘ μ§μ ν  μ μμ.

> widthμ heightμ μμ±κ°

| μ’λ₯     | μ€λͺ                                                                           |
| :------- | :----------------------------------------------------------------------------- |
| <ν¬κΈ°>   | λλΉκ° λμ΄μ κ°μ pxμ΄λ em λ¨μλ‘ μ§μ                                        |
| <λ°±λΆμ¨> | λ°μ€ λͺ¨λΈμ ν¬ν¨νλ λΆλͺ¨ μμλ₯Ό κΈ°μ€μΌλ‘ λλΉκ°μ΄λ λμκ°μ λ°±λΆμ¨(%)λ‘ μ§μ  |
| auto     | λ°μ€ λͺ¨λΈμ λλΉκ°κ³Ό λμκ°μ΄ μ½νμΈ μ μμ λ°λΌ μλμΌλ‘ κ²°μ , κΈ°λ³Έκ°         |

λ§μΌ widthμ heightλ‘ μ§μ ν μ½νμΈ  μμ­λ³΄λ€ μ€μ  μ½νμΈ κ° ν¬λ©΄ μ½νμΈ  μμ­μ λμΉκ² λλ€.
`overflow:hidden;`μ μ§μ νλ©΄ λμΉ μ½νμΈ λ₯Ό κ°μΆ μ μλ€.

widthμ height νλ‘νΌν°λ₯Ό λΉλ‘―ν λͺ¨λ  λ°μ€λͺ¨λΈ κ΄λ ¨ νλ‘νΌν°(margin, padding, border, box-sizing λ±)λ μμλμ§ μλλ€.

---

## 3. margin / padding νλ‘νΌν°

λ°μ€ λͺ¨λΈμ μνμ’μ° 4κ°μ λ°©ν₯μ΄ μμ΄μ νλλ¦¬λ λ§μ§, ν¨λ© λ±μ μ§μ ν  λ νκΊΌλ²μ λκ°μ΄ μ§μ νκ±°λ, λͺ¨λ λ€λ₯΄κ² μ§μ ν  μ μμ.

![margin / padding](../image/CSS/margin%3Apadding.png)

`margin`, `padding`μ -top, -right, -bottom, -left 4λ°©ν₯μ νλ‘νΌν°λ₯Ό κ°κ°μ§μ νμ§ μκ³  ν λ²μ μ§μ ν  μ μμ.

- 4κ°μ κ°μ μ§μ ν  λ - `margin: 25px 50px 75px 100px;`
  - μμλλ‘ top, right, bottom, left
- 3κ°μ κ°μ μ§μ ν  λ - `margin: 25px 50px 75px;`
  - μμλλ‘ top, right/left, bottom
- 2κ°μ κ°μ μ§μ ν  λ - `margin: 25px 50px;`
  - μμλλ‘ top/bottom, right/left
- 1κ°μ κ°μ μ§μ ν  λ - `margin: 25px;`
  - top/bottom/right/leftλ₯Ό λͺ¨λ κ°μ κ°μΌλ‘ μ§μ 

## 4. border νλ‘νΌν°

---

### 4-1. border-style

`border-style` νλ‘νΌν°λ νλλ¦¬μ μ μ μ€νμΌμ μ§μ , `border-style`4-1 border-style νλ‘νΌν°μ κΈ°λ³Έκ°μ noneμ΄λ―λ‘ μμ±κ°μ λ°λ‘ μ§μ νμ§ μμΌλ©΄ νλλ¦¬ μμμ΄λ λκ»λ₯Ό μ§μ νλλ νλ©΄μ νμλμ§ μμ.

[MDN: border-style](https://developer.mozilla.org/ko/docs/Web/CSS/border-style)

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      background-color: aquamarine;
      padding: 20;
      width: 320px;
      margin: 20px 0px;
      padding: 20px;
      text-align: center;
    }
    .dotted {
      border-style: dotted;
    }
    .dashed {
      border-style: dashed;
    }
    .solid {
      border-style: solid;
    }
    .double {
      border-style: double;
    }
    .groove {
      border-style: groove;
    }
    .ridge {
      border-style: ridge;
    }
    .inset {
      border-style: inset;
    }
    .outset {
      border-style: outset;
    }
    .none {
      border-style: none;
    }
    .hidden {
      border-style: hidden;
    }
    .mix {
      border-style: dotted dashed solid double;
    }
  </style>
</head>
<body>
  <div class="dotted">dotted</div>
  <div class="dashed">dashed</div>
  <div class="solid">solid</div>
  <div class="double">double</div>
  <div class="groove">groove</div>
  <div class="ridge">ridge</div>
  <div class="inset">inset</div>
  <div class="outset">outset</div>
  <div class="none">none</div>
  <div class="hidden">hidden</div>

  <!-- νλ‘νΌν° κ°μ κ°―μμ λ°λΌ 4κ° λ°©ν₯(top, right, left, bottom)μ λνμ¬ μ§μ μ΄ κ°λ₯νλ€. -->
  <div class="mix">dotted dashed solid double</div>
</body>
```

![border-style](../image/CSS/borderStyle.png)

---

### 4-2. border-width

`border-width` νλ‘νΌν°λ νλλ¦¬μ λκ»λ₯Ό μ§μ , νλ‘νΌν° κ°μ κ°―μμ λ°λΌ 4κ° λ°©ν₯(top, right, left, bottom)μ λνμ¬ μ§μ μ΄ κ°λ₯(margin, paddingκ³Ό κ°μ)

[MDN:border-width](https://developer.mozilla.org/ko/docs/Web/CSS/border-width)

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      background-color: aquamarine;
      padding: 20;
      width: 320px;
      margin: 20px 0px;
      padding: 20px;
      text-align: center;
      border-style: solid;
    }
    .one {
      border-width: 10px;
    }
    .two {
      border-width: 10px 20px;
    }
    .three {
      border-width: 10px 20px 30px;
    }
    .four {
      border-width: 10px 20px 30px 40px;
    }
  </style>
</head>
<body>
  <div class="one">10px</div>
  <div class="two">10px 20px</div>
  <div class="three">10px 20px 30px</div>
  <div class="four">10px 20px 30px 40px</div>
</body>
```

![border-width](../image/CSS/borderWidth.png)

---

### 4-3. border-color

`border-color` νλ‘νΌν°λ νλλ¦¬μ μμμ μ§μ , νλ‘νΌν° κ°μ κ°―μμ λ°λΌ 4κ° λ°©ν₯(top, right, left, bottom)μ λνμ¬ μ§μ μ΄ κ°λ₯(margin, paddingκ³Ό κ°μ)

[MDN: border-color](https://developer.mozilla.org/ko/docs/Web/CSS/border-color)

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      background-color: aquamarine;
      padding: 20;
      width: 320px;
      margin: 20px 0px;
      padding: 20px;
      text-align: center;
      border-style: solid;
      border-width: 10px;
    }
    .one {
      border-color: red;
    }
    .two {
      border-color: red green;
    }
    .three {
      border-color: red green blue;
    }
    .four {
      border-color: red green blue yellow;
    }
  </style>
</head>
<body>
  <div class="one">red</div>
  <div class="two">red green</div>
  <div class="three">red green blue</div>
  <div class="four">red green blue yellow</div>
</body>
```

![border-color](../image/CSS/borderColor.png)

---

### 4-4. border-radius

`border-radius` νλ‘νΌν°λ νλλ¦¬ λͺ¨μλ¦¬λ₯Ό λ₯κΈκ² νννλλ‘ μ§μ , νλ‘νΌν° κ°μ κΈΈμ΄λ₯Ό λνλ΄λ λ¨μ(px, emλ±)κ³Ό %λ₯Ό μ¬μ©, κ°κ°μ λͺ¨μλ¦¬μ border-radius νλ‘νΌν°λ₯Ό κ°λ³μ μΌλ‘ μ§μ ν  μλ μκ³  4κ°μ λͺ¨μλ¦¬λ₯Ό short-handλ‘ νλ²μ μ§μ  ν  μλ μμ

κΈ°λ³Έν `border-radius: <ν¬κΈ°> | <λ°±λΆμ¨>`

[MDN: border-radius](https://developer.mozilla.org/en-US/docs/Web/CSS/border-radius)

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      background-color: aquamarine;
      padding: 20;
      width: 160px;
      height: 160px;
      margin: 20px 0px;
      padding: 20px;
      text-align: center;
      border-style: solid;
      border-width: 1px;
      display: inline-block;
    }
    .one {
      /* 4κ°μ κΌ­μ§μ μ λν΄ Radius μ§μ  */
      border-radius: 10px;
    }
    .two {
      border-radius: 50%;
    }
    .three {
      /*  top-left & bottom-right | top-right & bottom-left */
      border-radius: 10px 50px;
    }
  </style>
</head>
<body>
  <div class="one">10px</div>
  <div class="two">50%</div>
  <div class="three">10px 50px</div>
</body>
```

![border-radius](../image/CSS/borderRadius.png)

> λκ°μ λ°μ§λ¦μ μ§μ νμ¬ νμν λ₯κ·Ό λͺ¨μλ¦¬ μ€μ νκΈ°

border-radius νλ‘νΌν°λ₯Ό μ¬μ©ν΄μ κΌ­μ§μ μ νμ ννλ‘ λ§λ€ μ μλ€. νλμ κ°(λ°μ§λ¦) λμ  νμμ κ°λ‘ λ°μ§λ¦κ°κ³Ό μΈλ‘ λ°μ§λ¦κ°μ /λ₯Ό ν΅ν΄ κ΅¬λΆνμ¬ λ£λλ€.  
κΈ°λ³Έν `border-radius: <κ°λ‘ λ°μ§λ¦> / <μΈλ‘ λ°μ§λ¦>`  
κΈ°λ³Έν `border-μμΉ-radius: <κ°λ‘ λ°μ§λ¦> / <μΈλ‘ λ°μ§λ¦>`

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      background-color: aquamarine;
      padding: 20;
      width: 320px;
      margin: 20px 0px;
      padding: 20px;
      text-align: center;
      border-style: solid;
      border-width: 1px;
      display: inline-block;
    }
    .one {
      border-radius: 30px;
    }
    .two {
      border-radius: 30px / 50px;
    }
  </style>
</head>
<body>
  <div class="one">border-radius: 30px</div>
  <div class="two">border-radius: 30px / 50px</div>
</body>
```

![border-radius2](../image/CSS/borderRadius2.png)

---

### 4-5. border

`border` νλ‘νΌν°λ `border-width`, `border-style`, `border-color`λ₯Ό νλ²μ μ€μ νκΈ° μν shorthand νλ‘νΌν°μ΄λ€.

[MDN: Border Shorthand](https://developer.mozilla.org/en-US/docs/Web/CSS/border)

κΈ°λ³Έν `border: border-width border-style border-color`

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      width: 320px;
      padding: 20px;
      border: 2px solid brown;
    }
  </style>
</head>
<body>
  <div class="one">border shorthand</div>
</body>
```

![border shorthand](../image/CSS/borderShorthand.png)

---

## 5. box-sizing νλ‘νΌν°

`box-sizing` νλ‘νΌν°λ width, height νλ‘νΌν°μ λμ μμ­μ λ³κ²½νλ€.

| ν€μλ      | μ€λͺ                                                                                                       |
| :---------- | :--------------------------------------------------------------------------------------------------------- |
| content-box | width, height νλ‘νΌν° κ°μ content μμ­μ μλ―Έ(κΈ°λ³Έκ°)                                                    |
| border-box  | width, height νλ‘νΌν° κ°μ content μμ­, padding, borderκ° ν¬ν¨λ κ°μ μλ―Έ, CSS Layoutμ μ§κ΄μ μΌλ‘ μ¬μ© |

![box-sizing](../image/CSS/boxSizing.png)

```html
<!DOCTYPE html>
<head>
  <style>
    div {
      width: 320px;
      padding: 20px;
      border: 2px solid brown;
      margin-bottom: 10px;
    }
    .border-box {
      box-sizing: border-box;
    }
  </style>
</head>
<body>
  <div class="content-box">content-box</div>
  <div class="border-box">border-box</div>
</body>
```

![box-sizing2](../image/CSS/boxSizing2.png)

> box-sizing νλ‘νΌν°λ μμλμ§ μλλ€. λ°λΌμ box-sizing νλ‘νΌν°λ₯Ό μ¬μ©νλλ‘ μ΄κΈ°ννλ €λ©΄ μλμ κ°μ΄ μ μνλ€.

```css
html {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: inherit;
}
```

---

## 6. Conclusion

> margin, padding, borderλ μ λ§ μΉλμμΈμ ν  λ flex, gridλ³΄λ€ λ λ§μ΄ μ¬μ©μ νλ νλ‘νΌν°λΌκ³  μκ°νλ€. κ·Έ λ§νΌ λ§μ΄ μ¬μ©νμΌλ ν΄λΉ λ΄μ©μ λν΄ μ΅μν΄μ‘μ§λ§ μ΄λ² μ±ν°λ₯Ό μ λ¦¬νλ©΄μ λͺ°λλ λΆλΆλ λ§μ΄ μκ² λμλ€.  
> μ§κΈκΉμ§ `border-radius: top-left top-right bottom-right bottom-left`λ₯Ό μ¬μ©νμ§ μκ³  μΌμΌν νλνλ κ°λ€μ μ§μ νλ€. μμΌλ‘λ ν μ€λ‘ νννλ λ°©λ²μΌλ‘ μ¬μ©μ ν΄μΌκ² λ€. λν νμνμΌλ‘ λ§λλ λ°©λ²μ λν΄μλ μ΄λ²μ μλ‘­κ² μκ² λμλ€. xμ’ν, yμ’νμ κ°κ° λ€λ₯Έ λ°μ§λ¦μ μ£Όμ΄ νμνμΌλ‘ λ§λλ λ°©λ²μΌλ‘ λ λ©μ§ μΉλμμΈμ ν  μ μλλ‘ νμ.  
> box-sizingκ°λμ λν΄μλ μ§κΈκΉμ§λ λ­κ° λ°μ€κ° μ΄μνλ°? νλ©΄μ μλ¬΄λ° μκ°μμ΄ `box-sizing: border-box`μ¬μ©ν΄μλ€. νμ§λ§ κ°λμ λν΄μ μ νν νμμ νλ μμ΄ μμν κΈ°λΆμ΄ λ€κ³  λκ΅°κ°μκ² λ°μ€ λͺ¨λΈμ λν΄ μ€λͺν  λ νμΈ΅ λ μμ κ°μκ² λλ΅ν  μ μμκ±° κ°λ€.π  
> μ¬μ©νλ cssλ§ κ³μ μ¬μ©νλ κ²λ³΄λ€ λ§μ κ²λ€μ λ°°μ°κ³  νλμ© μ μ©ν΄λ³΄λ©΄μ κ³΅λΆνμ

---

## μ°Έκ³ 

[poiemaweb 2-4 λ°μ€ λͺ¨λΈ](https://poiemaweb.com/css3-box-model)  
λμ - HTML + CSS + μλ°μ€ν¬λ¦½νΈ μΉ νμ€μ μ μ

---

[π](#box-model-λ°μ€-λͺ¨λΈ)
