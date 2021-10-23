2021-02-03 17:33
#mathSQL
# Математические функции SQL
[](https://docs.microsoft.com/ru-ru/sql/t-sql/functions/mathematical-functions-transact-sql?view=sql-server-ver15)
```sql
GREATEST(значение_1, значение_2) #  возвращает наибольшее значение
```
```sql 
								'MAX && MIN'
# SELECT MAX(поле) FROM имя_таблицы WHERE условие
SELECT MAX(salary) as max, MIN(salary) as min FROM workers
# MAX () с добавлением двух столбцов
SELECT MAX (opening_amt+receive_amt) 
FROM customer;
#
SELECT name_student, result 
FROM student 
JOIN attempt  
    ON student.student_id = attempt.student_id 
    AND result = (SELECT MAX(result) FROM attempt)
```
=> `AVG` возвращает среднее арифметическое по всем найденным записям [](http://old.code.mu/sql/avg.html)
=> `RAND()` генерация случайных чисел (от 0 до 1) `RAND() * 365` (от 0 до 365)
```SQL
SELECT question_id, name_question
FROM question
ORDER BY RAND() # Случайная выборка из базы данных
LIMIT 3
-------------------------------------------------------------
order by attempt_id desc, rand() # сортировка + рандом
```
=> `CEILING(x)`возвращает наименьшее целое число, большее или равное **x** `CEILING(4.2)=5`,  `CEILING(-5.8)=-5`   (CEILING-потолок)
=> `ROUND(x, k)`округляет значение **x** до **k** знаков после запятой,  
если **k** не указано – **x** округляется до целого `ROUND(4.361)=4`
=> `FLOOR(x)`возвращает наибольшее целое число, меньшее или равное **x** `FLOOR(4.2)=4`, `FLOOR(-5.8)=-6`
=> `POWER(x, y)`возведение **x** в степень **y**
=> `SQRT(x)` квадратный корень из **x**
=> `DEGREES(x)`конвертирует значение **x** из радиан в градусы `DEGREES(3) = 171.8...`
=> `RADIANS(x)`конвертирует значение **x** из градусов в радианы
=> `ABS(x)`модуль числа **x**
=> `PI()`pi = 3.1415926...
`COUNT(столбец)`                    - количество значений в столбце 
`COUNT(DISTINCT столбец)` - количество уникальных значений в столбце
```sql
# ПРИМЕР
SELECT price, price2, stepen, coren # какие столбцы получим 
	POWER(price, 2) AS stepen,
	SQRT(x) AS coren
FROM name_table;
```

Для повторного использования значения, можно использовать подзапрос по псевдониму.
```sql
SELECT a - b AS Разница,

       (SELECT(Разница)) + c AS Сумма
```
_____________
#### Links
[[SQL]] [[Выборка данных таблицы SQL]]