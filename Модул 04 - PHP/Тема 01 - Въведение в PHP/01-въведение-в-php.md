# Въведение в PHP

## Какво е PHP?

**PHP** (PHP: Hypertext Preprocessor) е сървърен скриптов език, използван за изграждане на динамични уеб страници. PHP кодът се изпълнява на **сървъра**, а резултатът (HTML) се изпраща до браузъра.

```
Браузър  ──→  HTTP заявка  ──→  Web сървър (Apache)
                                      │
                                 PHP интерпретатор
                                      │
                                 Генериран HTML
                                      │
Браузър  ←──  HTTP отговор  ←──  Web сървър
```

## Инсталиране на локален сървър (XAMPP)

1. Изтеглете и инсталирайте [XAMPP](https://www.apachefriends.org/)
2. Запомнете пътя на инсталацията
3. Стартирайте **xampp-control** и включете модулите **Apache** и **MySQL**

> ⚠️ Модулите се стартират **всеки път**, когато искате да използвате сървъра.

### Къде се създават сайтовете?

В директорията **`htdocs`** вътре в папката на XAMPP. Файловете там се зареждат в браузъра на адрес `http://localhost/`.

### Първи PHP файл

Създайте папка `MyWebsite` в `htdocs` и файл `index.php`:

```php
<!DOCTYPE html>
<html lang="bg">
<head>
    <meta charset="UTF-8">
    <title>Мой сайт</title>
</head>
<body>
    <?php
        echo "Hello World";
    ?>
</body>
</html>
```

Отворете `http://localhost/MyWebsite/` в браузъра.

## Синтаксис

### PHP тагове

```php
<?php
    // PHP код тук
?>
```

PHP може да се смесва с HTML:

```php
<h1><?php echo "Заглавие"; ?></h1>
<p><?= "Кратък синтаксис за echo" ?></p>
```

> `<?= $var ?>` е съкратен запис на `<?php echo $var; ?>`

### Коментари

```php
<?php
// Едноредов коментар
# Друг едноредов коментар

/*
   Многоредов
   коментар
*/
?>
```

## Променливи и типове

PHP е **динамично типизиран** — не се декларира тип на променливата.

```php
<?php
$number = 5;             // int
$numberString = '10';    // string
$greeting = 'Hello';     // string
$pi = 3.14;              // float
$isAdmin = true;         // bool
$empty = null;            // null
?>
```

### Конкатенация

В PHP стринговете се слепват с **точка** (`.`), не с `+`:

```php
<?php
$greeting = 'Hello';
$name = 'Student';
echo $greeting . ' ' . $name;  // "Hello Student"
?>
```

### Аритметика с типове

```php
<?php
$number = 5;
$numberString = '10';
echo $number + $numberString;  // 15 (PHP конвертира стринга в число)
?>
```

## Масиви

```php
<?php
// Индексиран масив
$colors = ["red", "green", "blue"];
echo $colors[0];  // "red"

// Асоциативен масив
$user = [
    "name" => "Ivan",
    "age" => 25,
    "city" => "Sofia"
];
echo $user["name"];  // "Ivan"

// Добавяне
$colors[] = "yellow";

// Брой елементи
echo count($colors);  // 4
?>
```

### Обхождане

```php
<?php
$fruits = ["apple", "banana", "cherry"];

// foreach
foreach ($fruits as $fruit) {
    echo $fruit . "<br>";
}

// с ключ и стойност
foreach ($user as $key => $value) {
    echo "$key: $value<br>";
}

// for цикъл
for ($i = 0; $i < count($fruits); $i++) {
    echo $fruits[$i] . "<br>";
}
?>
```

## Условни конструкции

```php
<?php
$age = 20;

if ($age >= 18) {
    echo "Пълнолетен";
} elseif ($age >= 16) {
    echo "Почти пълнолетен";
} else {
    echo "Непълнолетен";
}
?>
```

## Суперглобални масиви

PHP предоставя вградени масиви с данни от заявката:

| Масив | Описание |
|-------|----------|
| `$_GET` | Данни от URL параметри |
| `$_POST` | Данни от POST форма |
| `$_REQUEST` | Комбинация от GET и POST |
| `$_SERVER` | Информация за сървъра и заявката |
| `$_SESSION` | Данни за текущата сесия |
| `$_COOKIE` | Бисквитки |
| `$_FILES` | Качени файлове |

### Пример: Получаване на данни от форма

```html
<form action="process.php" method="POST">
    <input type="text" name="username" placeholder="Потребителско име">
    <input type="password" name="password" placeholder="Парола">
    <button type="submit">Изпрати</button>
</form>
```

```php
<?php
// process.php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST["username"];
    $password = $_POST["password"];
    echo "Потребител: " . htmlspecialchars($username);
}
?>
```

> ⚠️ **Винаги** използвайте `htmlspecialchars()` при извеждане на потребителски вход, за да предотвратите XSS атаки.
