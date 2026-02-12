# Box Model

## Какво е Box Model?

Всеки HTML елемент в CSS е правоъгълна „кутия", състояща се от 4 слоя:

```
┌──────────────────────────────────────┐
│              margin                  │
│  ┌────────────────────────────────┐  │
│  │           border               │  │
│  │  ┌──────────────────────────┐  │  │
│  │  │        padding           │  │  │
│  │  │  ┌──────────────────┐   │  │  │
│  │  │  │    content        │   │  │  │
│  │  │  └──────────────────┘   │  │  │
│  │  └──────────────────────────┘  │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
```

| Слой | Описание |
|------|----------|
| **content** | Съдържанието — текст, изображения |
| **padding** | Вътрешно разстояние между съдържанието и рамката |
| **border** | Рамка около елемента |
| **margin** | Външно разстояние между елемента и съседните елементи |

## Padding (вътрешно отстояние)

```css
.box {
    padding: 20px;              /* еднакво от всички страни */
    padding: 10px 20px;         /* горе/долу 10, ляво/дясно 20 */
    padding: 10px 20px 15px 5px; /* горе, дясно, долу, ляво */
}

/* Или поотделно: */
.box {
    padding-top: 10px;
    padding-right: 20px;
    padding-bottom: 15px;
    padding-left: 5px;
}
```

## Border (рамка)

```css
.box {
    border: 2px solid #333;
    border-radius: 8px;         /* заоблени ъгли */
}

/* Само определена страна: */
.box {
    border-bottom: 3px dashed red;
}
```

Стилове на рамка: `solid`, `dashed`, `dotted`, `double`, `none`.

## Margin (външно отстояние)

```css
.box {
    margin: 20px;
    margin: 0 auto;            /* центриране по хоризонтала */
}

/* Поотделно: */
.box {
    margin-top: 10px;
    margin-bottom: 20px;
}
```

> **Margin collapse:** Когато два вертикални margin-а се допират, те не се сумират, а се взема по-големият.

## Box Sizing

По подразбиране `width` и `height` задават размера само на **content**. Padding и border се добавят отгоре.

```css
/* По подразбиране */
.box {
    width: 300px;
    padding: 20px;
    border: 5px solid;
    /* Реална ширина = 300 + 20*2 + 5*2 = 350px */
}

/* С box-sizing: border-box ширината включва padding и border */
.box {
    box-sizing: border-box;
    width: 300px;
    padding: 20px;
    border: 5px solid;
    /* Реална ширина = 300px (content се свива автоматично) */
}
```

> **Добра практика:** Добавете в началото на всеки CSS файл:
> ```css
> *, *::before, *::after {
>     box-sizing: border-box;
> }
> ```

## Ширина и височина

```css
.box {
    width: 300px;
    max-width: 100%;       /* не надвишава контейнера */
    min-height: 200px;     /* минимална височина */
    height: auto;          /* височина спрямо съдържанието */
}
```

### Единици за мерене

| Единица | Описание |
|---------|----------|
| `px` | Пиксели — фиксирана стойност |
| `%` | Процент от родителя |
| `em` | Спрямо размера на шрифта на родителя |
| `rem` | Спрямо размера на шрифта на `<html>` |
| `vw` / `vh` | Процент от ширината / височината на viewport |

## Display

```css
.block { display: block; }       /* заема цял ред */
.inline { display: inline; }     /* заема колкото му трябва */
.ib { display: inline-block; }   /* inline + width/height */
.hidden { display: none; }       /* скрит елемент */
```

| Стойност | Поведение |
|----------|-----------|
| `block` | Нов ред, заема пълна ширина (`div`, `p`, `h1`) |
| `inline` | На същия ред, без width/height (`span`, `a`, `strong`) |
| `inline-block` | На същия ред, но с width/height |
| `none` | Скрива елемента напълно |

## Position

```css
.static { position: static; }      /* по подразбиране */
.relative { position: relative; top: 10px; left: 20px; }
.absolute { position: absolute; top: 0; right: 0; }
.fixed { position: fixed; top: 0; width: 100%; }
.sticky { position: sticky; top: 0; }
```

| Стойност | Описание |
|----------|----------|
| `static` | Нормален поток (по подразбиране) |
| `relative` | Отместване спрямо нормалната позиция |
| `absolute` | Позициониране спрямо най-близкия `relative`/`absolute` родител |
| `fixed` | Фиксиран спрямо viewport-а (не се мести при скрол) |
| `sticky` | Комбинация — нормален, докато не достигне определена позиция при скрол |
