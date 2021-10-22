2021-02-03 18:07
#tag
# Логические функции

В ***`PostgreSQL`*** вместо `IF`  используют конструкцию `CASE ... WHEN ... THEN ... ELSE ... END` и [некоторые другие](https://www.postgresql.org/docs/12/functions-conditional.html).

```sql
CASE
	WHEN author = 'Text 1' THEN price * 2
	WHEN author = 'Text 2' THEN price * 3
	ELSE price END

SELECT *,
  CASE
  WHEN (column1 IS NULL OR column2 = 'Hello world!') AND (author IN ('Маяковский', 'Донская')) THEN 'Первый'
  ELSE 'Второй'
  END AS case_when_else
FROM your_table;
```
В ***`SQL`*** реализована возможность заносить в поле значение в зависимости от условия. Для этого используется функция `IF`:
```sql
IF(логическое_выражение, выражение_1, выражение_2)
```
```sql
IFNULL(выражение, значение) # выражение - проверяется на NULL, 
							# значение - возвращается, если выражение равно NULL.
COALESCE(A,B) вернет A, если B - NULL и вернет B, если  A - NULL.
# Вместо IF() можно также воспользоваться функцией COALESCE(), которая возвращает первое отличное от NULL значение из списка своих аргументов.
``` 
Функция вычисляет `логическое_выражение,` если оно истина – в поле заносится значение `выражения_1`, в противном случае –  значение `выражения_2.` Все три параметра `IF()` являются обязательными.

Допускается использование вложенных функций, вместо `выражения_1` или `выражения_2` может стоять новая функция `**IF**`.
```sql
SELECT title, amount, price, 
    IF(amount<4, price*0.5, price*0.7) AS sale # если кол-во<4, 0.5 от цены, иначе 0.7 от цены ЗАПИСЫВАЕМ(AS) как sale
FROM book;
```
![[if_sql.png]]
**Пример**
Если количество книг меньше 4 – то скидка 50%, меньше 11 – 30%, в остальных случаях – 10%. И еще укажем какая именно скидка на каждую книгу. Добим округление до 2-х знаков ROYND(what, vol)
```sql
SELECT title, amount, price,
    ROUND(IF(amount < 4, price  *0.5, IF(amount < 11, price * 0.7, price * 0.9)), 2) AS sale,
    IF(amount < 4, 'скидка 50%', IF(amount < 11, 'скидка 30%', 'скидка 10%')) AS Ваша_скидка
FROM book;
```
![[if_sql1.png]]
### Альтернатива IF - When
```SQL
select author, title, 
round(case author
    when "Булгаков М.А." then price * 1.1
    when "Есенин С.А." then price * 1.05
    else price end, 2) as new_price
from book
```
### Выборка данных по условию
```sql
SELECT коллонка1, column2 				# выбрать колонки на вывод
FROM table_name							# из какой таблицы
WHERE column (условие[<,>,=,..]) vol;	# условие выбора WHERE price < 600;
# сочетание нескольких условий (AND= &&, OR= ||, NOT= !=)
WHERE column (условие[<,>,=,..]) vol OR column (условие[<,>,=,..]) vol
WHERE (условие < vol || условие > vol) && условие * условие >= vol;
```
_____________
#### Links
[[SQL]] [[Выборка данных таблицы SQL]]