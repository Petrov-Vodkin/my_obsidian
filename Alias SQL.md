2021-04-02 15:15
#alias 
# Alias SQL
#### Использование временного имени таблицы (алиаса)
Для присваивания псевдонима существует 2 варианта: 
```sql
'1'# -   с использованием ключевого слова AS 
FROM fine AS f, traffic_violation AS tv
'2'# -   а так же и без него
FROM fine f, traffic_violation tv
```
_____________
#### Links
[[SQL]]