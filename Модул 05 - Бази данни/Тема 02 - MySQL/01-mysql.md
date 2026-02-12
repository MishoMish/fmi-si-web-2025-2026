# MySQL

## Достъп до MySQL

Чрез XAMPP имате достъп до phpMyAdmin:

```
http://localhost/phpmyadmin
```

Там можете да създавате бази данни, таблици и да изпълнявате SQL заявки.

## Създаване на база данни

В phpMyAdmin: **Databases** → въведете име → **Create**.

Или чрез SQL:

```sql
CREATE DATABASE myfirstdatabase;
USE myfirstdatabase;
```

## Създаване на таблица

```sql
CREATE TABLE users (
    id INT(11) NOT NULL AUTO_INCREMENT,
    username VARCHAR(30) NOT NULL,
    pwd VARCHAR(255) NOT NULL,
    email VARCHAR(100) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (id)
);
```

### Обяснение

| Част | Описание |
|------|----------|
| `INT(11)` | Цяло число |
| `NOT NULL` | Не може да е празно |
| `AUTO_INCREMENT` | Автоматично увеличаващ се номер |
| `VARCHAR(255)` | Текст до 255 символа |
| `DEFAULT CURRENT_TIMESTAMP` | Стойност по подразбиране — текущо време |
| `PRIMARY KEY (id)` | `id` е уникален идентификатор |

## INSERT — Вмъкване на данни

```sql
INSERT INTO users (username, pwd, email)
VALUES ('antonia', 'tony1234', 'antonia1235@abv.bg');
```

Множество записи:

```sql
INSERT INTO users (username, pwd, email) VALUES
    ('ivan', 'pass123', 'ivan@abv.bg'),
    ('maria', 'pass456', 'maria@abv.bg');
```

## SELECT — Извличане на данни

```sql
-- Всички колони
SELECT * FROM users;

-- Конкретни колони
SELECT username, email FROM users;

-- С условие
SELECT * FROM users WHERE username = 'antonia';

-- Сортиране
SELECT * FROM users ORDER BY created_at DESC;

-- Ограничаване
SELECT * FROM users LIMIT 10;

-- Броене
SELECT COUNT(*) FROM users;
```

### WHERE условия

```sql
SELECT * FROM users WHERE age > 18;
SELECT * FROM users WHERE age BETWEEN 18 AND 30;
SELECT * FROM users WHERE email LIKE '%@abv.bg';
SELECT * FROM users WHERE city IN ('Sofia', 'Plovdiv');
SELECT * FROM users WHERE name IS NOT NULL;
```

| Оператор | Описание |
|----------|----------|
| `=` | Равно на |
| `!=` или `<>` | Различно от |
| `>`, `<`, `>=`, `<=` | Сравнения |
| `LIKE` | Съвпадение по шаблон (`%` = произволен текст) |
| `IN` | В списък от стойности |
| `BETWEEN` | В диапазон |
| `IS NULL` / `IS NOT NULL` | Проверка за NULL |
| `AND`, `OR` | Логически оператори |

## UPDATE — Обновяване

```sql
UPDATE users SET username = 'new_name' WHERE id = 1;
UPDATE users SET pwd = 'newpass', email = 'new@abv.bg' WHERE id = 1;
```

> ⚠️ **Винаги** добавяйте `WHERE`! Без `WHERE` ще обновите **всички** записи.

## DELETE — Изтриване

```sql
DELETE FROM users WHERE id = 5;
```

> ⚠️ Без `WHERE` ще изтриете **всички** записи!

## JOIN — Свързване на таблици

```sql
-- Вземи поръчките заедно с имената на потребителите
SELECT users.username, orders.amount, orders.created_at
FROM orders
INNER JOIN users ON orders.user_id = users.id;
```

| Тип JOIN | Описание |
|----------|----------|
| `INNER JOIN` | Само записи с съвпадения в двете таблици |
| `LEFT JOIN` | Всички от лявата + съвпадащи от дясната |
| `RIGHT JOIN` | Всички от дясната + съвпадащи от лявата |

## Пример: Пълна схема

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    name VARCHAR(50) NOT NULL,
    age INT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```
