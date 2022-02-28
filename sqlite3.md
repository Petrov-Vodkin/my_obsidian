2021-10-12 10:44
[link_pythonist](https://pythonist.ru/rabota-s-bazoj-dannyh-modul-sqlite3/)
# sqlite3
SQLite -  обладает всеми необходимыми функциями реляционной базы данных, но все сохраняется в одном файле. На [официальном сайте](https://www.sqlite.org/index.html) представлены несколько сценариев использования SQLite:

-   Встраиваемые системы и Интернет вещей.
-   Анализ данных.
-   Передача данных.
-   Файловый архив и/или контейнер данных.
-   Внутренние или временные базы данных.
-   Замена базы данных во время демонстрации или тестирования.
-   Обучение и тестирование.
-   Экспериментальные расширения языка SQL.
```py
import sqlite3 as sl
```
### Просмотр БД
`DB Browser SQLite`
Доступна консольная утилита для работы с базами (`sqlite3`.exe, «a command-line shell for accessing and modifying SQLite databases»)[.](https://habr.com/ru/post/149356/)
#### Консольная утилита 
[Основные команды](https://ruseller.com/lessons.php?id=2277)
**`Мета Команды`** - предназначены для формирования таблиц и других административных операций

|Команда | Описание |
|--------|----------|
|.show |Показывает текущие настройки заданных параметров 
|.databases |Показывает название баз данных и файлов 
|.quit |Выход из sqlite3 
|.tables |Показывает текущие таблицы 
|.schema |Отражает структуру таблицы 
|.header |Отобразить или скрыть шапку таблицы 
|.mode |Выбор режима отображения данных таблицы 
|.dump |Сделать копию базы данных в текстовом формате 

```sql
CREATE TABLE comments (
    post_id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT NOT NULL,
    website_url TEXT NULL,
    comment TEXT NOT NULL );
```
```sql
INSERT INTO comments ( name, email, website_url, comment )
VALUES ( 'Shivam Mamgain', 'xyz@gmail.com',
'shivammg.blogspot.com', 'Great tutorial for beginners.' );
```
```sql
SELECT post_id, name, email, website_url, comment
FROM comments;
```
```sql
UPDATE comments
SET email = 'zyx@email.com'
WHERE name = 'Shivam Mamgain';
```
```sql
DELETE FROM comments
WHERE post_id = 9;
```
```sql
# добавления новой колонки - введём поле username 
ALTER TABLE comments
ADD COLUMN username TEXT;

# переименования таблицы `comments` на `Coms`
ALTER TABLE comments
RENAME TO Coms;
```
```sql
DROP TABLE Coms;
```
_______________________________
### Создание подключения к БД

Не беспокойтесь о драйверах, строках подключения и так далее. Вы можете создать базу данных SQLite и получить объект подключения к ней (Connection) следующим образом:
```py
con = sl.connect('my-test.db')
```
После выполнения этой строки кода мы уже создали базу данных и подключились к ней. Это происходит потому, что база данных, к которой мы попросили Python подключиться, не существует, так что автоматически создается пустая. В противном случае мы можем использовать точно такой же код для подключения к существующей базе данных.

![модуль sqlite3](https://miro.medium.com/max/700/1*rviROzOMi1ymOQ4DvLgDsQ.png)

### Создание таблицы

Далее давайте создадим таблицу.
```py
with con:
    con.execute("""
        CREATE TABLE USER (
            id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
            name TEXT,
            age INTEGER
        );
    """)
```

В таблицу `USER` мы добавили три столбца — id, name, age. Как видите, SQLite  поддерживает все основные функции обычной СУБД, такие как тип данных, nullable, первичный ключ и автоинкремент.

После выполнения этого кода будет создана таблица.

### Вставка записей

Давайте вставим несколько записей в только что созданную таблицу `USER`, чтобы убедиться, что мы действительно ее создали.

Предположим, мы хотим вставить несколько записей за один раз. В Python мы можем легко это сделать:
```py
sql = 'INSERT INTO USER (id, name, age) values(?, ?, ?)'
data = [
    (1, 'Alice', 21),
    (2, 'Bob', 22),
    (3, 'Chris', 23)
]
```
Нам нужно определить запрос SQL с вопросительными знаками (`?`) для подстановки значений. Затем создать тестовые данные для вставки. После этого, с помощью объекта подключения, мы можем вставить наши тестовые строки.
```py
with con:
	con.executemany(sql, data)
```
### Запросы

Пришло время проверить все, что мы сделали. Давайте выполним запрос к таблице, чтобы получить тестовые строки.

```py
with con:
    data = con.execute("SELECT * FROM USER WHERE age <= 22")
    for row in data:
        print(row)
```
![модуль sqlite3](https://miro.medium.com/max/584/1*j1IilyztcKJsfgPdlT-92w.png)

Как видите, все очень просто.
_____________
#### Links
[[sql]]