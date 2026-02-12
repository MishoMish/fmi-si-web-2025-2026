# Web PHP — HTTP, формуляри и сесии

## Как работи уеб заявката?

```
1. Потребителят отваря URL в браузъра
2. Браузърът изпраща HTTP заявка (GET/POST) към сървъра
3. Apache получава заявката и предава .php файла на PHP интерпретатора
4. PHP изпълнява кода и генерира HTML
5. Сървърът връща HTML като HTTP отговор
6. Браузърът рендерира страницата
```

## HTTP методи

| Метод | Описание | Кога се използва |
|-------|----------|------------------|
| `GET` | Извличане на данни | Зареждане на страница, търсене |
| `POST` | Изпращане на данни | Регистрация, логин, създаване |
| `PUT` / `PATCH` | Обновяване на данни | Промяна на профил |
| `DELETE` | Изтриване | Изтриване на запис |

## Обработка на формуляри

### HTML формуляр

```html
<form action="register.php" method="POST">
    <input type="text" name="username" placeholder="Username">
    <input type="email" name="email" placeholder="Email">
    <input type="password" name="pwd" placeholder="Password">
    <button type="submit">Submit</button>
</form>
```

### PHP обработка

```php
<?php
// register.php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST["username"];
    $email = $_POST["email"];
    $pwd = $_POST["pwd"];

    // Валидация
    if (empty($username) || empty($email) || empty($pwd)) {
        echo "Всички полета са задължителни!";
        exit;
    }

    // Обработка...
    echo "Регистрацията е успешна!";
} else {
    // Ако не е POST — пренасочване
    header("Location: index.php");
    exit;
}
?>
```

## Пренасочване

```php
<?php
header("Location: index.php");
die(); // или exit; — спира изпълнението след пренасочване
?>
```

> `header()` трябва да се извика **преди** какъвто и да е HTML изход.

## GET параметри (Query String)

URL: `user.php?key1=value1&key2=value2`

```php
<?php
$key1 = $_GET["key1"];  // "value1"
$key2 = $_GET["key2"];  // "value2"
?>
```

```html
<form action="user.php?key1=value1&key2=value2" method="POST">
    <!-- POST данните са в $_POST, GET параметрите в $_GET -->
</form>
```

## Сесии

Сесиите позволяват съхранение на данни между различни заявки (HTTP е stateless).

### Стартиране

```php
<?php
session_start(); // ТРЯБВА да е преди всякакъв HTML изход
?>
```

### Записване и четене

```php
<?php
session_start();

// Записване
$_SESSION['user_id'] = 42;
$_SESSION['username'] = "Ivan";

// Четене
echo $_SESSION['username']; // "Ivan"

// Проверка
if (isset($_SESSION['user_id'])) {
    echo "Потребителят е логнат";
}
?>
```

### Унищожаване

```php
<?php
session_start();
session_destroy();
?>
```

## JSON отговори от PHP

PHP може да работи като API, връщайки JSON:

```php
<?php
header('Content-Type: application/json');

$data = ['status' => 'SUCCESS', 'message' => 'Операцията е успешна'];
echo json_encode($data);
?>
```

### Получаване на JSON от заявка

```php
<?php
$inputData = json_decode(file_get_contents("php://input"), true);
$email = $inputData['email'] ?? '';
$password = $inputData['password'] ?? '';
?>
```

## Routing с switch

Един файл обработва различни HTTP методи:

```php
<?php
// session.php
require_once 'bootstrap.php';

header('Content-Type: application/json');

switch ($_SERVER['REQUEST_METHOD']) {
    case 'GET':
        // Проверка дали потребителят е логнат
        $isLoggedIn = Session::isUserLoggedIn();
        echo json_encode(['loggedIn' => $isLoggedIn]);
        break;

    case 'POST':
        // Логин
        $email = $_POST['email'];
        $password = $_POST['password'];

        $connection = (new Db())->getConnection();
        $stmt = $connection->prepare("SELECT * FROM users WHERE email = :email");
        $stmt->execute(['email' => $email]);
        $userData = $stmt->fetch();

        $success = $userData && password_verify($password, $userData['password']);

        if ($success) {
            Session::logUser(User::fromArray($userData));
        }

        echo json_encode(['success' => $success]);
        break;

    case 'DELETE':
        // Логаут
        session_destroy();
        echo json_encode(['success' => true]);
        break;
}
?>
```

## Сигурност

### XSS предотвратяване

```php
<?php
// НИКОГА не извеждайте потребителски вход директно:
echo $_POST['name']; // ❌ Опасно!

// Винаги escape-вайте:
echo htmlspecialchars($_POST['name'], ENT_QUOTES, 'UTF-8'); // ✅
?>
```

### SQL Injection предотвратяване

```php
<?php
// НИКОГА не вмъквайте директно в SQL:
$query = "SELECT * FROM users WHERE email = '$email'"; // ❌ Опасно!

// Винаги използвайте prepared statements:
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = :email");
$stmt->execute(['email' => $email]); // ✅
?>
```
