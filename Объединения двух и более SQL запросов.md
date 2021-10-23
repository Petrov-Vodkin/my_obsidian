2021-04-17 16:19
#tag
# UNION
```sql
SELECT столбец_1_1, столбец_1_2, ...
FROM 
  ...
UNION						# `UNION` удаляет повторяющиеся записи
SELECT столбец_2_1, столбец_2_2, ...
FROM 
  ...
										'OR'
SELECT столбец_1_1, столбец_1_2, ...
FROM 
  ...
UNION ALL					# ALL в результат включаются все записи запросов
SELECT столбец_2_1, столбец_2_2, ...
FROM 
  ...
```
[ПРИМЕР](https://stepik.org/lesson/308891/step/14?unit=291017)
```sql
SELECT buy_id, client_id, book_id, date_payment, amount, price
FROM 
    buy_archive
UNION ALL
SELECT buy.buy_id, client_id, book_id, date_step_end, buy_book.amount, price
FROM 
    book 
    INNER JOIN buy_book USING(book_id)
    INNER JOIN buy USING(buy_id) 
    INNER JOIN buy_step USING(buy_id)
    INNER JOIN step USING(step_id)                  
WHERE  date_step_end IS NOT Null and name_step = "Оплата"
ORDER BY client_id
```
_____________
#### Links
[[SQL]]