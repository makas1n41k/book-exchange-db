# 📚 Лабораторна робота з PostgreSQL

## 🔹 Опис проєкту

Цей проєкт містить приклад створення реляційної бази даних у PostgreSQL для системи обліку книг, користувачів та їхніх оцінок/вражень.
База даних реалізує зв’язки між таблицями `user`, `books`, `activity`, `book_impressions`, а також демонструє використання **PRIMARY KEY, FOREIGN KEY, UNIQUE, CHECK** та **ON DELETE CASCADE**.

## 🔹 Структура бази даних

* **user** — зберігає інформацію про користувачів (ім’я, логін, Telegram-нік).
* **books** — каталог книг із назвою, автором та жанром.
* **activity** — фіксує факт взаємодії користувача з книгою (зв’язує `user` і `books`).
* **book_impressions** — зберігає оцінки та коментарі користувачів до книг.

Зв’язки:

* Один користувач може взаємодіяти з багатьма книгами.
* Для кожної взаємодії можуть зберігатися кілька відгуків.
* При видаленні користувача або книги дані автоматично каскадно видаляються.

## 🔹 Як запустити

1. Встанови PostgreSQL (наприклад, через [pgAdmin4](https://www.pgadmin.org/) або CLI).
2. Створи нову базу даних, наприклад:

   ```sql
   CREATE DATABASE library_db;
   ```
3. Виконай SQL-скрипт з файлу `schema.sql` (цей файл містить `CREATE TABLE ...`).
4. Перевір наявність створених таблиць у pgAdmin чи psql:

   ```sql
   \dt
   ```

## 🔹 Тестові дані

Приклад вставки кількох тестових записів:

```sql
INSERT INTO "user"(full_name, username, tg_handle)
VALUES 
('Anna Petrenko', 'apetrenko', '@anna_p');

INSERT INTO books(title, author, genre)
VALUES
('Тіні забутих предків', 'Михайло Коцюбинський', 'Novel');

INSERT INTO activity(user_id, book_id) VALUES (1, 1);

INSERT INTO book_impressions(user_id, book_id, rating, comment)
VALUES (1, 1, 4.5, 'Цікава книга, варта прочитання');
```

## 🔹 Перевірка каскадного видалення

* Видалення користувача автоматично прибирає всі його записи з `activity` та `book_impressions`.
* Видалення книги прибирає всі згадки про неї в інших таблицях.

Приклад:

```sql
DELETE FROM "user" WHERE user_id = 1;
-- автоматично видалить записи з activity і book_impressions
```

