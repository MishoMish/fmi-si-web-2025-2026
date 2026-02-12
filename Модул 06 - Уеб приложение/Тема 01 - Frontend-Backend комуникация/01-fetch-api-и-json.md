# Frontend–Backend комуникация

## Как фронтендът говори с бекенда?

В класическите уеб приложения HTML формите изпращат данни директно и страницата се презарежда. В съвременните приложения **JavaScript** изпраща заявки на заден план (асинхронно) чрез **Fetch API** и обновява само нужната част от страницата.

```
┌────────────────┐         HTTP          ┌────────────────┐
│                │  ──── GET/POST ────→  │                │
│    Браузър     │                       │   PHP сървър   │
│  (HTML + JS)   │  ←──── JSON ───────  │   (Apache)     │
│                │                       │                │
└────────────────┘                       └────────────────┘
```

## Fetch API

### GET заявка

```js
fetch('./session.php', {
    method: 'GET',
    headers: {
        'Accept': 'application/json'
    }
})
.then(response => response.json())
.then(data => {
    console.log(data);
})
.catch(error => {
    console.error('Грешка:', error);
});
```

### POST заявка с JSON

```js
const data = {
    email: 'test@abv.bg',
    password: '1234'
};

fetch('./session.php', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
})
.then(response => response.json())
.then(result => {
    console.log(result);
});
```

### POST заявка с FormData

```js
const form = document.querySelector('form');

form.addEventListener('submit', (event) => {
    event.preventDefault();

    let formData = new FormData(event.target);

    fetch('./session.php', {
        method: 'POST',
        body: formData  // Content-Type се задава автоматично
    })
    .then(response => response.json())
    .then(result => {
        console.log(result);
    });
});
```

### DELETE заявка

```js
fetch('./session.php', {
    method: 'DELETE'
})
.then(response => response.json())
.then(result => {
    console.log(result); // { success: true }
});
```

## async / await

По-четим синтаксис за асинхронни операции:

```js
async function login(email, password) {
    try {
        const response = await fetch('./session.php', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password })
        });

        const result = await response.json();

        if (result.success) {
            window.location.href = 'homepage.html';
        } else {
            document.getElementById('error').textContent = 'Грешен логин!';
        }
    } catch (error) {
        console.error('Мрежова грешка:', error);
    }
}
```

### В addEventListener

```js
form.addEventListener('submit', async (e) => {
    e.preventDefault();

    const res = await fetch('includes/login_user.php', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            email: form.email.value,
            password: form.pwd.value
        })
    });

    const result = await res.json();
    document.getElementById('response').textContent = result.message;

    if (result.status === 'SUCCESS') {
        window.location.href = 'homepage.html';
    }
});
```

## JSON — JavaScript Object Notation

JSON е текстов формат за обмен на данни между клиент и сървър.

```json
{
    "status": "SUCCESS",
    "message": "Успешен вход!",
    "user": {
        "id": 1,
        "name": "Иван"
    }
}
```

### В JavaScript

```js
// Обект → JSON стринг
let jsonString = JSON.stringify({ name: "Ivan", age: 25 });

// JSON стринг → Обект
let obj = JSON.parse('{"name":"Ivan","age":25}');
```

### В PHP

```php
<?php
// Масив → JSON
echo json_encode(['status' => 'OK', 'data' => [1, 2, 3]]);

// JSON → Масив
$data = json_decode(file_get_contents("php://input"), true);
?>
```

## PHP като API endpoint

```php
<?php
// api.php
header('Content-Type: application/json');

switch ($_SERVER['REQUEST_METHOD']) {
    case 'GET':
        echo json_encode(['loggedIn' => isset($_SESSION['user_id'])]);
        break;

    case 'POST':
        $data = json_decode(file_get_contents("php://input"), true);
        // обработка...
        echo json_encode(['success' => true]);
        break;

    case 'DELETE':
        session_destroy();
        echo json_encode(['success' => true]);
        break;

    default:
        http_response_code(405);
        echo json_encode(['error' => 'Method not allowed']);
}
?>
```

## HTTP статус кодове

| Код | Значение | Кога се използва |
|-----|----------|------------------|
| `200` | OK | Успешна заявка |
| `201` | Created | Създаден нов ресурс |
| `400` | Bad Request | Невалидни входни данни |
| `401` | Unauthorized | Липсва автентикация |
| `403` | Forbidden | Нямате права |
| `404` | Not Found | Ресурсът не е намерен |
| `405` | Method Not Allowed | HTTP методът не е поддържан |
| `500` | Internal Server Error | Грешка в сървъра |

```php
<?php
http_response_code(400);
echo json_encode(['error' => 'Невалидни данни']);
?>
```

## Проверка на отговора в JS

```js
const response = await fetch('/api/users');

if (!response.ok) {
    // Статус код 4xx или 5xx
    const error = await response.json();
    console.error(`Грешка ${response.status}: ${error.message}`);
    return;
}

const data = await response.json();
// Обработка на успешен отговор...
```
