2021-04-12 17:40
#tag
# Соединение, использование USING()
`USING` позволяет указать набор столбцов, которые __`есть в обеих объединяемых таблицах`__. Если база данных хорошо спроектирована, а каждый внешний ключ имеет такое же имя, как и соответствующий первичный ключ (например, `genre.genre_id = book.genre_id`), тогда можно использовать предложение `USING` для реализации операции `JOIN`.
```sql
							"Пример с ON"
SELECT title, name_author, author.author_id /* явно указать таблицу - обязательно */
FROM author
	INNER JOIN book
    ON author.author_id = book.author_id;
```
Вывести название книг, фамилии и **`id`** их авторов.
```sql
							"Вариант c USING"
SELECT title, name_author, author_id /* имя таблицы, из которой берется author_id, указывать не обязательно*/
FROM author
	INNER JOIN book
    USING(author_id);
```

[](https://stepik.org/lesson/308886/step/9?thread=solutions&unit=291012)
```sql
# Если в таблицах supply  и book есть одинаковые книги,  вывести их название и автора
SELECT book.title, name_author
FROM 
    author 
    INNER JOIN book USING (author_id)   
    INNER JOIN supply ON book.title = supply.title 
                         and author.name_author = supply.author;
```
Схема данных
![](https://ucarecdn.com/6de130b3-aac2-4216-82bb-7421c2a614ad/)
```sql
"_Результат:_"
+----------------+-------------+------------+
| Название       | Автор       | Количество |
+----------------+-------------+------------+
| Черный человек | Есенин С.А. | 12         |
+----------------+-------------+------------+
```
_____________
#### Links
[[SQL]]