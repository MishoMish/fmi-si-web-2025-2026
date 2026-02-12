# Flexbox

## Какво е Flexbox?

**Flexbox** (Flexible Box Layout) е CSS модул за ефективно подравняване и разпределяне на елементи в контейнер. Това е **едномерна система** — контролира подредбата по **ред (row)** или **колона (column)**.

## Основна структура

```html
<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
</div>
```

```css
.container {
    display: flex;
    background: #f5f5f5;
}

.item {
    flex: 1;
    padding: 20px;
    background: lightblue;
    margin: 5px;
}
```

## Свойства на Flex контейнера

### `display: flex`

Превръща елемента във flex контейнер. Децата му стават flex елементи.

### `flex-direction`

Определя посоката на главната ос:

```css
flex-direction: row;            /* ← по подразбиране: отляво надясно */
flex-direction: row-reverse;    /* отдясно наляво */
flex-direction: column;         /* отгоре надолу */
flex-direction: column-reverse; /* отдолу нагоре */
```

### `justify-content`

Подравнява елементите по **главната ос**:

```
row:    ←————————————————→  (хоризонтална)
column: ↑                   (вертикална)
        |
        ↓
```

```css
justify-content: flex-start;    /* в началото (по подразбиране) */
justify-content: center;        /* в средата */
justify-content: flex-end;      /* в края */
justify-content: space-between; /* равно разстояние между елементите */
justify-content: space-around;  /* равно разстояние около елементите */
justify-content: space-evenly;  /* напълно равно разстояние */
```

### `align-items`

Подравнява елементите по **напречната ос**:

```css
align-items: stretch;    /* разтягане (по подразбиране) */
align-items: flex-start; /* в началото */
align-items: flex-end;   /* в края */
align-items: center;     /* в средата */
align-items: baseline;   /* по базовата линия на текста */
```

### `flex-wrap`

Контролира дали елементите се пренасят на нов ред:

```css
flex-wrap: nowrap;       /* всичко на един ред (по подразбиране) */
flex-wrap: wrap;         /* пренасяне на нов ред */
flex-wrap: wrap-reverse; /* пренасяне в обратна посока */
```

### `gap`

Разстояние между flex елементите:

```css
.container {
    display: flex;
    gap: 10px;            /* еднакво навсякъде */
    gap: 10px 20px;       /* ред / колона */
}
```

### Комбиниран пример

```css
.container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 10px;
}
```

## Свойства на Flex елементите

### `flex-grow`

Определя колко да се **разшири** елементът спрямо останалите:

```css
.item-a { flex-grow: 1; }  /* 1 част */
.item-b { flex-grow: 2; }  /* 2 части (двойно по-широк) */
```

### `flex-shrink`

Определя как елементите се **свиват** при ограничено пространство:

```css
.item { flex-shrink: 1; }  /* свива се пропорционално */
.item { flex-shrink: 0; }  /* НЕ се свива */
```

### `flex-basis`

Начален размер на елемента преди разпределяне на свободното пространство:

```css
.item { flex-basis: 200px; }
.item { flex-basis: 30%; }
```

### Съкратен запис `flex`

```css
.item { flex: 1; }              /* grow:1, shrink:1, basis:0 */
.item { flex: 0 0 200px; }      /* фиксиран 200px, без растене/свиване */
.item { flex: 1 1 auto; }       /* расте и се свива от автоматичен размер */
```

### `order`

Контролира визуалния ред (по подразбиране `0`):

```css
.item:first-child { order: 3; }  /* показва се последен */
.item:last-child  { order: -1; } /* показва се първи */
```

### `align-self`

Пренаписва `align-items` за конкретен елемент:

```css
.special {
    align-self: flex-end;  /* само този елемент е долу */
}
```

## Пример: Центриране на съдържание

```html
<div class="center-box">
    <p>Здравей, Flexbox!</p>
</div>
```

```css
.center-box {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 200px;
    background: #ddd;
}
```

## Пример: Навигационно меню

```css
.nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 20px;
    background: #333;
}

.nav a {
    color: white;
    text-decoration: none;
    padding: 5px 10px;
}

.nav a:hover {
    background: #555;
    border-radius: 4px;
}
```

## Пример: Два панела (30% / 70%)

```css
.layout {
    display: flex;
    min-height: 100vh;
}

.sidebar {
    flex: 0 0 30%;
    background: #f0f0f0;
    padding: 20px;
}

.main-content {
    flex: 1;       /* заема останалото пространство */
    padding: 20px;
}
```

## Полезни ресурси

- [A Complete Guide to Flexbox (CSS-Tricks)](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [Flexbox Froggy (интерактивна игра)](https://flexboxfroggy.com/)
