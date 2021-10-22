2021-07-10 15:12
#tag
# Mutable Immutable
|	Неименяемые типы		|		 Изменяемые типы	|
|-----------------|---------------|
|	Числа					|	Список `list` |
|	Строки `str`			| 	Словарь `dict`|
|	Булевые значения `True False` |Множества `set {}` |
|	Кортеж `tuple ()`		| Массивы байтов `bytearrays`|
|	frozenset-ы 			|	
|	байты					| 
```py
Копия `list` (изменяемого объекта) 
x = [1, 2, 3]
y = x[:] 		# ссылаемся на полный СРЕЗ списка
y = x.copy()
```
_____________
#### Links
https://techrocks.ru/2020/11/11/mutable-immutable-objects-in-python/