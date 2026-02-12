# ООП в PHP

## Класове и обекти

### Деклариране на клас

```php
<?php
class User {
    // Свойства (properties)
    private $id;
    private $email;
    private $name;
    private $age;

    // Конструктор
    public function __construct($id, $email, $name, $age) {
        $this->id = $id;
        $this->email = $email;
        $this->name = $name;
        $this->age = $age;
    }

    // Методи
    public function getName() {
        return $this->name;
    }

    public function getEmail() {
        return $this->email;
    }

    public function getId() {
        return $this->id;
    }

    public function getAge() {
        return $this->age;
    }
}
?>
```

### Създаване на обект

```php
<?php
$user = new User(1, "ivan@abv.bg", "Иван", 25);
echo $user->getName();  // "Иван"
echo $user->getAge();   // 25
?>
```

## Достъпност (Visibility)

| Модификатор | Описание |
|-------------|----------|
| `public` | Достъпно отвсякъде |
| `private` | Достъпно само в класа |
| `protected` | Достъпно в класа и наследниците |

```php
<?php
class Account {
    public $name;          // достъпно отвсякъде
    private $balance;      // само в класа
    protected $currency;   // в класа и наследниците

    public function getBalance() {
        return $this->balance;  // OK — вътре в класа
    }
}

$acc = new Account();
echo $acc->name;     // OK
echo $acc->balance;  // Грешка! private
?>
```

> **Добра практика:** Свойствата да са `private` с public getter/setter методи.

## Статични методи и свойства

```php
<?php
class Session {
    public static function logUser(User $user): void {
        $_SESSION['user_id'] = $user->getId();
    }

    public static function isUserLoggedIn(): bool {
        return isset($_SESSION['user_id']);
    }
}

// Извикване без обект:
Session::logUser($user);
$isLogged = Session::isUserLoggedIn();
?>
```

## Фабричен метод (Factory)

```php
<?php
class User {
    private $id;
    private $email;
    private $name;
    private $age;

    public function __construct($id, $email, $name, $age) {
        $this->id = $id;
        $this->email = $email;
        $this->name = $name;
        $this->age = $age;
    }

    // Създаване от асоциативен масив
    public static function fromArray(array $data): User {
        return new User(
            $data['id'],
            $data['email'],
            $data['name'],
            $data['age']
        );
    }
}

// Използване:
$userData = ['id' => 1, 'email' => 'test@abv.bg', 'name' => 'Test', 'age' => 30];
$user = User::fromArray($userData);
?>
```

## Хеширане на пароли

```php
<?php
class User {
    private $hashedPassword;

    public function setPassword(string $password): void {
        $this->hashedPassword = password_hash($password, PASSWORD_DEFAULT);
    }

    public function getHashedPassword(): string {
        return $this->hashedPassword;
    }
}

// Проверка на парола при логин:
$isCorrect = password_verify($inputPassword, $storedHash);
?>
```

> ⚠️ **Никога** не съхранявайте пароли в чист текст! Винаги използвайте `password_hash()` и `password_verify()`.

## Autoloading (автоматично зареждане)

Вместо ръчно `require` за всеки клас:

```php
<?php
// bootstrap.php
spl_autoload_register(function ($className) {
    $classDirs = ["classes", "classes/models", "classes/controllers"];

    foreach ($classDirs as $dir) {
        $filePath = "./{$dir}/{$className}.php";
        if (file_exists($filePath)) {
            require_once $filePath;
            return;
        }
    }
});
?>
```

С autoloader всеки `new User(...)` автоматично зарежда `classes/User.php`.

## Защита на файлове с `.htaccess`

Файловете с класове не трябва да са директно достъпни от браузъра:

```apache
# classes/.htaccess
Deny from all
```

## Примерна структура на проект

```
MyWebsite/
├── index.php           ← входна точка
├── bootstrap.php       ← autoloader, сесии, error handling
├── session.php         ← API за сесии (GET/POST/DELETE)
├── user.php            ← API за потребители
├── homepage.html        ← frontend
├── mystyle.css
├── myscript.js
├── classes/
│   ├── .htaccess       ← Deny from all
│   ├── Db.php          ← връзка с БД
│   ├── Session.php     ← управление на сесии
│   └── User.php        ← модел за потребител
└── php-templates/
    ├── head.php        ← преизползваем <head>
    └── profile.php     ← страница с шаблон
```
