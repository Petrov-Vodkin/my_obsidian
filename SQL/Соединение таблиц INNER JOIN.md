2021-04-08 15:22
#tag
# Соединение таблиц INNER JOIN
[[Выборка из нескольких таблиц SQL]]

`INNER JOIN` соединяет две таблицы. Порядок таблиц для оператора неважен, поскольку оператор является симметричным.[](https://stepik.org/lesson/308886/step/3?unit=291012)

```sql
SELECT столбец1, столбец2
 ...
FROM
    "таблица_1" INNER JOIN  "таблица_2" # без ковычек
    ON условие
...
```
Результат запроса формируется так:
-   каждая строка одной таблицы сопоставляется с каждой строкой второй таблицы;
-   для полученной «соединённой» строки проверяется условие соединения;
-   если условие истинно, в таблицу результата добавляется соответствующая «соединённая» строка; 
#### Внешнее соединение LEFT и RIGHT OUTER JOIN
```sql
SELECT
 ...
FROM
    таблица_1 LEFT JOIN  таблица_2
    ON условие
...
```
Результат запроса формируется так:
- в результат включается внутреннее соединение (`INNER JOIN`) первой и второй таблицы в соответствии с условием;
- затем в результат добавляются те записи первой таблицы, которые не вошли во внутреннее соединение на шаге 1, для таких записей соответствующие поля второй таблицы заполняются значениями `NULL`.

![[sqljoin.png]]
```sql
									"Пример"
# Вывести название книг и их авторов.
SELECT title, name_author
FROM 
    author INNER JOIN book
    ON author.author_id = book.author_id;
	'Поскольку поля `author_id` в таблицах `book` и `author` называются одинаково,
	необходимо в запросах указывать полную ссылку на них (`book.author_id` и `author.author_id`).'
```

```sql
+-----------------------+------------------+
| title                 | name_author      |
+-----------------------+------------------+
| Мастер и Маргарита    | Булгаков М.А.    |
| Белая гвардия         | Булгаков М.А.    |
| Идиот                 | Достоевский Ф.М. |
| Братья Карамазовы     | Достоевский Ф.М. |
| Игрок                 | Достоевский Ф.М. |
| Стихотворения и поэмы | Есенин С.А.      |
| Черный человек        | Есенин С.А.      |
| Лирика                | Пастернак Б.Л.   |
+-----------------------+------------------+
```

#### Запросы на выборку из нескольких таблиц
```sql
SELECT name_genre, title, name_author
FROM genre g 
    INNER JOIN book b ON g.genre_id=b.genre_id
    INNER JOIN author a ON a.author_id=b.author_id
WHERE LOWER(name_genre) = 'роман'
ORDER BY title
```
#### Вложенные запросы в операторах соединения
```sql
SELECT
 ...
FROM
    таблица ... JOIN  
       (
        SELECT ...
       ) имя_вложенного_запроса
    ON условие
...
```
```sql
SELECT  name_author
FROM author INNER JOIN book
     on author.author_id = book.author_id
GROUP BY name_author, genre_id
HAVING genre_id IN 
         (/* выбираем автора, если он пишет книги в самых популярных жанрах*/
          SELECT query_in_1.genre_id
          FROM 
              ( /* выбираем код жанра и количество произведений, относящихся к нему */
                SELECT genre_id, SUM(amount) AS sum_amount
                FROM book
                GROUP BY genre_id
               )query_in_1
          INNER JOIN 
              ( /* выбираем запись, в которой указан код жанр с максимальным количеством книг */
                SELECT genre_id, SUM(amount) AS sum_amount
                FROM book
                GROUP BY genre_id
                ORDER BY sum_amount DESC
                LIMIT 1
               ) query_in_2
          ON query_in_1.sum_amount= query_in_2.sum_amount
         );              
  
```
_____________
#### Links
[[SQL]]