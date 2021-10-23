2021-02-02 17:11
#tabl #cозданиетаблицы
# Создание таблицы
указывается какая таблица создается, из каких атрибутов(полей) она состоит и какой тип данных имеет каждое поле, при необходимости указывается описание полей (ключевое поле и т.д.). Его структура :
-   ключевые слова : `CREATE TABLE`
-   имя создаваемой таблицы(`book`);
-   название поля(`book_id`& `name_book`) 
-   описание полей(`INT PRIMARY KEY AUTO_INCREMENT`&& `VARCHAR(30)`), которое включает тип поля (`INT`|| `VARCHAR`|| `DEC`)и другие необязательные характеристики;
```sql
CREATE TABLE book(							# название таблтцы == book
    book_id INT PRIMARY KEY AUTO_INCREMENT, # название ТИП ОПИСАНИЕ ХАРАК_КИ,
    name_author VARCHAR(30)					# name_author (строка длинной 30) 
	some_num DECIMAL(8,2),					# число
	some_date DATE							# дата
);
```
Созданная таблица - пустая.
```sql
						"Вывести строки созданой таблицы"
DESCRIBE table_name;
# OR
SHOW COLUMNS FROM table_name;
```

## Вставка записи в таблицу
```sql
INSERT INTO таблица(поле1, поле2) 
VALUES (значение1, значение2);

# title выставляется автоматом(т.к PRIMARY KEY AUTO_INCREMENT)
INSERT INTO book (title, author, price, amount)
VALUES
# или INSERT INTO book VALUES
    ('Белая гвардия', 'Булгаков М.А.', 540.50 , 5),
    ('Идиот', 'Достоевский Ф.М.', 460, 10),
    ('Братья Карамазовы', 'Достоевский Ф.М.', 799.01, 2);
```

```sql
SELECT * FROM tabl_name; #  Вывести всю таблицу tabl_name (показать)
```
## Создать таблицу на основе запроса
 Создать таблицу на основе основе другой таблицы
 ```sql
CREATE TABLE ordering AS		# создвём таблицу КАК
SELECT author, title, 			# выбор строк автор, книга,
   (
    SELECT ROUND(AVG(amount)) 	# , округлённое среднее количество книг
    FROM book					# ИЗ другой таблицы book
   ) AS amount
FROM book						# ИЗ другой таблицы book 
WHERE amount < 4;				# где количество(в таблие book) < 4
```
```sql
CREATE TABLE buy_pay AS
SELECT title, name_author, price, bb.amount,
       price*bb.amount Стоимость
  FROM book b
       JOIN buy_book bb ON (buy_id=5) AND (b.book_id=bb.book_id)
       JOIN author USING(author_id)
 ORDER BY title;
 ```
## Создание таблицы с внешними ключами
```sql
# каждый внешний ключ должен иметь такой же тип данных, как связанное поле главной таблицы (в наших примерах это `INT`);
# необходимо указать главную для нее таблицу и столбец, по которому осуществляется связь:
FOREIGN KEY (связанное_поле_зависимой_таблицы)  
REFERENCES главная_таблица (связанное_поле_главной_таблицы)
"ПРИМЕР"
FOREIGN KEY (author_id)  REFERENCES author (author_id) 
```
По умолчанию любой столбец, кроме ключевого, может содержать значение `NULL`. При создании таблицы это можно переопределить,  используя  ограничение `NOT NULL` для этого столбца:

```sql
CREATE TABLE таблица (
    столбец_1 INT NOT NULL, 
    столбец_2 VARCHAR(10) 
);
# Для внешних ключей рекомендуется устанавливать ограничение `NOT NULL` (если это совместимо с другими опциями, которые будут рассмотрены в следующем шаге).
```

```sql
								"EXAMPLE"
CREATE TABLE book (
    book_id INT PRIMARY KEY AUTO_INCREMENT, 
    title VARCHAR(50), 
    author_id INT NOT NULL, # пустое значение не допускается
    price DECIMAL(8,2), 
    amount INT, 
	# внешний ключ:  главная таблица author,  связанный столбец author.author_id
    FOREIGN KEY (author_id)  REFERENCES author (author_id) 
);
```
___________
#### Links
[[SQL]]