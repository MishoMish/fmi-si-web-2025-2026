# Основи на CSS

## Какво е CSS?

**CSS** (Cascading Style Sheets) описва визуалното оформление на HTML документ. Използва се за контролиране на цветове, шрифтове, подредба, размери и разположение на елементите.

### Защо да използваме CSS?

- Разделя **структурата** (HTML) от **визуалното оформление** (CSS)
- Позволява лесна промяна на дизайна на множество страници от едно място
- Намалява повторенията и прави кода по-четим
- Предоставя повече контрол върху оформлението

## Начини за добавяне на CSS

### 1. Inline CSS

Директно в HTML елемент чрез атрибут `style`:

```html
<p style="color: blue; font-size: 18px;">Това е параграф.</p>
```

### 2. Internal CSS (вътрешен)

В `<style>` таг в секцията `<head>`:

```html
<head>
    <style>
        p {
            color: blue;
            font-size: 16px;
        }
    </style>
</head>
```

### 3. External CSS (външен) ✅ Препоръчителен

Във външен `.css` файл, свързан с `<link>`:

```html
<link rel="stylesheet" href="styles.css">
```

```css
/* styles.css */
p {
    color: blue;
    font-size: 16px;
}
```

## Синтаксис

```css
селектор {
    свойство: стойност;
    свойство: стойност;
}
```

```css
a {
    color: red;
    text-decoration: none;
}
```

## Коментари

```css
/* Това е коментар — не се вижда в браузъра */
h1 {
    color: green;
}
```

## Селектори

### По име на елемент (tag selector)

```css
p { color: blue; }
h1 { font-size: 24px; }
```

### По клас (class selector)

```css
.button { background-color: blue; color: white; }
.container { width: 800px; margin: 0 auto; }
```

```html
<button class="button">Натисни</button>
<div class="container">...</div>
```

### По ID (id selector)

```css
#header { background-color: #333; }
#main { padding: 20px; }
```

```html
<div id="header">...</div>
```

> ID се използва за **един конкретен** елемент. Класовете — за **много** елементи.

### Комбиниране на селектори

```css
/* Всички <p> вътре в елемент с клас .article */
.article p { color: gray; }

/* Директни деца */
.nav > li { display: inline; }

/* Елементи с два класа едновременно */
.button.primary { background: blue; }

/* Групиране — общи стилове */
h1, h2, h3 { font-family: Arial, sans-serif; }
```

### Псевдо-класове

```css
a:hover { color: orange; }
a:visited { color: purple; }
input:focus { border-color: blue; }
li:first-child { font-weight: bold; }
```

### Селектор по атрибут

```css
input[type="text"] { border: 1px solid gray; }
input[type="password"] { border: 1px solid red; }
a[target="_blank"] { color: green; }
```

### Псевдо-клас `nth-child`

```css
/* Всеки трети елемент */
#container div:nth-child(3n+1) { background-color: lightgray; }
#container div:nth-child(3n+2) { background-color: lightgreen; }
```

## Каскадност и специфичност

CSS означава *Cascading* Style Sheets — стиловете се „наслагват" и при конфликт браузърът избира правилото с по-висок приоритет.

### Йерархия на специфичност (от най-силен до най-слаб)

| Приоритет | Тип | Пример |
|-----------|-----|--------|
| 1 (най-силен) | `!important` | `color: red !important;` |
| 2 | inline style | `<p style="color:red">` |
| 3 | id | `#title {}` |
| 4 | class / pseudo-class | `.text {}`, `:hover` |
| 5 (най-слаб) | елемент | `p {}` |

### Пример

```css
p { color: blue; }
.text { color: green; }
#main { color: red; }
```

Елемент `<p id="main" class="text">` ще бъде **червен**, защото `id` има по-висока специфичност.

### `!important`

```css
p {
    color: blue !important;
}
```

> ⚠️ Използвайте `!important` само в краен случай — прави кода труден за поддръжка.

## Основни CSS свойства

### Цветове

```css
.text { color: red; }                     /* по име */
.text { color: #ff0000; }                 /* HEX */
.text { color: rgb(255, 0, 0); }          /* RGB */
.text { color: rgba(255, 0, 0, 0.5); }    /* RGBA (с прозрачност) */
```

### Шрифтове

```css
body {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 16px;
    font-weight: normal;     /* normal, bold, 100-900 */
    line-height: 1.5;
}
```

### Фон

```css
.box {
    background-color: #f5f5f5;
    background-image: url('bg.jpg');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
}
```

## Добри практики

✅ Използвай външни CSS файлове  
✅ Групирай сходни стилове  
✅ Използвай ясни имена на класове  
✅ Добавяй коментари при по-сложни части  

❌ Прекомерна употреба на `!important`  
❌ Inline стилове навсякъде  
❌ Дублирани правила  
❌ Неконсистентни единици (`px` / `em` / `rem`)
