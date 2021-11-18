2021-11-17 19:26
#tag
# functool
-Инструменты расширения функций и других вызываемых объектов.
-предназначен для [функций высшего порядка](https://docs-python.ru/tutorial/opredelenie-funktsij-python/peredacha-funktsii-drugoj-funktsii/ "Передача функции в качестве аргумента другой функции."): `функций, которые действуют или возвращают другие функции`. В общем, любой вызываемый объект может рассматриваться как [функция](https://docs-python.ru/tutorial/opredelenie-funktsij-python/ "Функции в Python, определение функций.") для целей этого модуля.

Модуль `functools` предоставляет инструменты для адаптации или расширения функций и других вызываемых объектов, не переписывая их полностью.

##### [Декоратор @cached_property модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-cached-property-modulja-functools/ "Кешировать метод класса и преобразовать его в свойство в Python.")

Декоратор @cached_property модуля functools преобразует метод класса в свойство, значение которого вычисляется один раз, а затем кэшируется как обычный атрибут в течение срока службы экземпляра.

##### [Функция cmp_to_key() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/funktsija-cmp-to-key-modulja-functools/ "Преобразовать функцию сравнения в ключевую функцию в Python.")

Класс `cmp_to_key()` модуля `functools` преобразует функцию сравнения старого стиля в ключевую функцию. Эта функция в основном используется в качестве инструмента перехода для программ, конвертируемых из Python 2.

##### [Декоратор @lru_cache() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-lru-cache-modulja-functools/ "Кеширование результата выполнения функции в Python.")

Декоратор `lru_cache()` модуля `functools` оборачивает функцию с переданными в нее аргументами и запоминает возвращаемый результат соответствующий этим аргументам.

##### [Декоратор @total_ordering модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-total-ordering-modulja-functools/ "Создать недостающие методы сравнения в Python.")

Декоратор класса `total_ordering()` модуля `functools` оборачивает класс, который определяет один или несколько методов сравнения и добавляет остальные.

##### [Функция partial() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/funktsija-partial-modulja-functools/ "Заморозить часть аргументов вызываемой функции в Python.")
Функция `partial()` используется для частичного применения каких то аргументов к вызываемой функции `func`. Другими словами `partial()` "замораживает" некоторую часть аргументов и/или ключевых слов, в результате чего создается новый объект с упрощенной записью аргументов вызываемой функции.
```py
from functools import partial  
  
x = int("1101", base=2)  	# 
print(x)  					# 13
int_2 = partial(int, base=2)# создаём ф-ю с "постоянным аргументом" base=2
x = int_2("1101")  			# задан по дефолту base=2
print(x)  					# 13
```
##### [Класс partialmethod() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/klass-partialmethod-modulja-functools/ "Заморозить часть аргументов метода класса в Python.")

Класс `partialmethod()` модуля `functools` возвращает новый дескриптор метода `func`, который ведет себя как функция `functools.partial()`, за исключением того, что он предназначен для использования в качестве определения метода, а не для прямого вызова.

##### [Функция reduce() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/funktsija-reduce-modulja-functools/ "Функция reduce() в Python.")

Функция `reduce()` модуля `functools` кумулятивно применяет функцию `function` к элементам итерируемой `iterable` последовательности, сводя её к единственному значению.

#####  [Декоратор @singledispatch модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-singledispatch-modulja-functools/ "Универсальная функция одиночной диспетчеризации в Python.")

Декоратор `@singledispatch` модуля `functools` создает из обычной функции - универсальную функцию одиночной диспетчеризации.

##### [Декоратор @singledispatchmethod модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-singledispatchmethod-modulja-functools/ "Универсальный метод одиночной диспетчеризации в Python.")

Декоратор `singledispatchmethod()` модуля `functools` создает из обычного метода класса - универсальный метод одиночной диспетчеризации.

##### [Декоратор @update_wrapper() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-update-wrapper-modulja-functools/ "Заменить атрибуты функции атрибутами другой функции в Python.")

Декоратор `@update_wrapper()` модуля `functools` обновляет функцию-обертку, чтобы она выглядела как исходная функция. Другими словами дополняет декоратор, данными из некоторых атрибутов оборачиваемой функции.

##### [Декоратор @wraps() модуля functools в Python.](https://docs-python.ru/standart-library/modul-functools-python/dekorator-wraps-modulja-functools/ "Заменить атрибуты декоратора на атрибуты исходной функции в Python.")

Декоратор `@wraps()` модуля `functools` это удобная функция для вызова `@functools.update_wrapper()` в качестве декоратора при определении функции-обертки.
_____________
#### Links
https://docs-python.ru/standart-library/modul-functools-python/