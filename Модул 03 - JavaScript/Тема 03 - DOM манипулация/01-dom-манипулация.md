# DOM манипулация

## Какво е DOM?

**DOM** (Document Object Model) е дървовидна структура, която браузърът създава от HTML документа. JavaScript може да чете и променя DOM дървото, за да прави страницата динамична.

```
document
└── html
    ├── head
    │   └── title
    └── body
        ├── h1
        ├── p
        └── div
            ├── p
            └── img
```

## Селектиране на елементи

### По ID

```js
let header = document.getElementById('header');
```

### По клас

```js
let items = document.getElementsByClassName('item');
// Връща HTMLCollection (подобен на масив)
```

### По CSS селектор (✅ препоръчително)

```js
// Първият съвпадащ елемент
let firstItem = document.querySelector('.item');

// Всички съвпадащи елементи (NodeList)
let allItems = document.querySelectorAll('.item');
```

| Метод | Връща | Пример |
|-------|-------|--------|
| `getElementById` | Един елемент | `getElementById('main')` |
| `getElementsByClassName` | HTMLCollection | `getElementsByClassName('btn')` |
| `querySelector` | Един елемент | `querySelector('#main .title')` |
| `querySelectorAll` | NodeList | `querySelectorAll('p.highlight')` |

## Промяна на съдържание

### `innerHTML` vs `textContent`

```js
let content = document.getElementById('content');

// innerHTML рендерира HTML тагове
content.innerHTML = "New <strong>HTML</strong> content";

// textContent променя само текста (HTML тагове се показват като текст)
content.textContent = "Just plain text";
```

> ⚠️ **innerHTML** може да е уязвим на XSS атаки ако вмъквате потребителски вход. Предпочитайте `textContent` когато не е нужен HTML.

## Промяна на стилове

```js
let element = document.getElementById('box');

element.style.color = "red";
element.style.backgroundColor = "#f0f0f0";
element.style.fontSize = "20px";
element.style.display = "none";
```

> CSS свойства с тире се пишат в **camelCase**: `background-color` → `backgroundColor`.

## Работа с класове

```js
let el = document.querySelector('.box');

el.classList.add('active');       // добавя клас
el.classList.remove('active');    // премахва клас
el.classList.toggle('active');    // добавя ако липсва, премахва ако има
el.classList.contains('active');  // true / false
```

## Работа с атрибути

```js
let link = document.querySelector('a');

link.getAttribute('href');                // четене
link.setAttribute('href', '/new-page');   // промяна
link.removeAttribute('target');           // премахване

// Data атрибути
let el = document.querySelector('.item');
el.setAttribute('data-selected', 'true');
el.getAttribute('data-selected');          // "true"
// Или чрез dataset:
el.dataset.selected = 'true';
```

## Създаване и премахване на елементи

### Създаване

```js
let newParagraph = document.createElement('p');
newParagraph.textContent = "Нов параграф";
newParagraph.classList.add('highlight');

// Добавяне в DOM
document.body.appendChild(newParagraph);

// Вмъкване преди друг елемент
let container = document.getElementById('content');
let firstChild = container.firstElementChild;
container.insertBefore(newParagraph, firstChild);
```

### Премахване

```js
let element = document.getElementById('old');
element.remove();

// Или чрез родител:
let parent = document.getElementById('container');
let child = document.getElementById('old');
parent.removeChild(child);
```

## Събития (Events)

### addEventListener

```js
let button = document.getElementById('myBtn');

button.addEventListener('click', function(event) {
    console.log('Бутонът беше натиснат!');
    console.log(event.target);  // елементът, който е натиснат
});
```

### Често използвани събития

| Събитие | Описание |
|---------|----------|
| `click` | Клик с мишката |
| `dblclick` | Двоен клик |
| `mouseover` / `mouseout` | Мишката влиза / излиза |
| `keydown` / `keyup` | Клавиш натиснат / отпуснат |
| `submit` | Изпращане на форма |
| `input` | Промяна в поле за въвеждане |
| `focus` / `blur` | Елементът получава / губи фокус |
| `DOMContentLoaded` | HTML-ът е зареден |

### Arrow функция в addEventListener

```js
button.addEventListener('click', () => {
    console.log('Clicked!');
});
```

### Обект на събитието (event)

```js
document.addEventListener('keydown', (event) => {
    console.log(`Натиснат клавиш: ${event.key}`);
});
```

## Действия по подразбиране

Спират се чрез `event.preventDefault()`:

```js
let form = document.querySelector('form');

form.addEventListener('submit', (event) => {
    event.preventDefault(); // спира изпращането

    let input = document.querySelector('#name').value;

    if (input.length < 3) {
        document.getElementById('error').textContent =
            "Името трябва да е поне 3 символа!";
    } else {
        document.getElementById('pass').textContent =
            `Здравей, ${input}!`;
    }
});
```

## Пълен пример: Toggle selection

```html
<div id="container">
    <div>one</div>
    <div>two</div>
    <div>three</div>
</div>
```

```css
#container div.selected {
    background-color: red;
    color: white;
}
```

```js
document.querySelectorAll('#container div').forEach(element => {
    element.addEventListener('click', (event) => {
        event.target.classList.toggle('selected');
    });
});
```

## Пълен пример: Показване / скриване на форма

```js
function revealLoginForm() {
    document.getElementById('login-form').style.display = 'block';
}

function hideLoginForm() {
    document.getElementById('login-form').style.display = 'none';
}

document.getElementById('login-button')
    .addEventListener('click', revealLoginForm);

document.getElementById('close-button')
    .addEventListener('click', hideLoginForm);
```
