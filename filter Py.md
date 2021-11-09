2021-11-09 14:25
[doc_py](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-filter/)
# filter Py
озволяет отфильтровать элементы последовательности по условию.
#### Синтаксис:
```py
filter(func, iterable)
```
#### Параметры:

-   func - [функция](https://docs-python.ru/tutorial/opredelenie-funktsij-python/ "Функции в Python, определение функций."), которая принимает элемент фильтруемого объекта и должна вернуть [`bool`](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/bool-logicheskij-tip-dannyh/ "bool, логический тип данных.") значение,
-   iterable - [последовательность](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tipy-posledovatelnostej/ "Типы последовательностей") или объект поддерживающий [итерирование](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-iterator-iterator/ "Тип данных Iterator (итератор).").

#### Возвращаемое значение:

filter object - отфильтрованная последовательность.

#### Описание:

Функция [`filter()`](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-filter/ "Функция filter() в Python, фильтрует список по условию.") отбирает/фильтрует элементы переданного объекта `iterable` при помощи пользовательской функции `func`.

Функция `filter()` принимает в качестве аргументов пользовательскую функцию и объект, элементы которого следует отфильтровать (может быть последовательностью или объектом поддерживающий итерирование).

-   Если фильтрующая функция `func` вернёт `False`, то элемент последовательности `iterable` не попадёт в результат выполнения функции `filter()`.
-   Если фильтрующая функция `func` вернет `None`, то считается что требуется применить тождественное действие `(item for item in iterable if item)`, таким образом все элементы, оцениваемые как `False` будут отфильтрованы.

Чтобы получить результат из элементов оцененных пользовательской функцией как `False`, воспользуйтесь [`itertools.filterfalse()`](https://docs-python.ru/standart-library/modul-itertools-python/funktsija-filterfalse-modulja-itertools/ "Функция filterfalse() модуля itertools в Python.").

Из всего сказанного можно сделать вывод, что для успешного использования [функции `filter()`](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-filter/ "Функция filter() в Python, фильтрует список по условию.") необходимо в результате каких то **вычислений/сравнений над каждым элементом** передаваемой последовательности получать `True` или 1 для элементов которые нужно отобрать и `False` или 0 для элементов, которые нужно отбросить.

При несложных вычислениях фильтрации, легче всего такое поведение можно получить при помощи [лямбда-функции](https://docs-python.ru/tutorial/opredelenie-funktsij-python/anonimnye-funktsii-lambda-vyrazhenija/ "Анонимные функции (lambda-выражения)  в Python"), используя в них стандартные функции или методы, возвращающие `bool` значения, [операции сравнения](https://docs-python.ru/tutorial/operatsii-sravnenija-python/ "Операторы/операции сравнения (цепочки сравнений) в Python."), оператор [вхождения `in`](https://docs-python.ru/tutorial/obschie-operatsii-posledovatelnostjami-list-tuple-str-python/prinadlezhnost-elementa-stroki-posledovatelnosti/ "Проверка существования значения в последовательности Python.") и [оператор идентичности `is`](https://docs-python.ru/tutorial/operator-identichnosti-is-is-not-python/ "Оператор идентичности is в Python.").

Если необходимо произвести более сложные вычисления фильтрации, то для этого необходимо определить [обычную функцию](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-filter/#func) и передать ее в качестве первого аргумента функции `filter()`.

## Примеры фильтрации списков по условию.

-   [Применение лямбда-функции в качестве фильтра](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-filter/#lambda);
-   [Применение обычной функции в качестве фильтра](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-filter/#func).

### Применение лямбда-функции в качестве фильтра последовательности.
```py
# имеем числовую последовательность
num = [1, 2.0, 3.1, 4, 5, 6, 7.9]

# использование операции сравнения
# отбираем числа > 4
>>> f = filter(lambda x: x > 4, num)
>>> list(f)
# [5, 6, 7.9]

# использование оператора идентичности `is`
# отбираем только целые числа
>>> f = filter(lambda x: type(x) is int, num)
>>> list(f)
# [1, 4, 5, 6]

# теперь работаем со списком строк
num = ['ноль', '1', '2.0', '3.1', '4', '5', '7.9', 'семь']

# использование встроенных функций или методов
# возвращающих 'bool' значения на примере метода 'str.isdigit()'
# отбираем строки с записью чисел
>>> f = filter(lambda x: x.replace('.', '').isdigit(), num)
>>> list(f)
# ['1', '2.0', '3.1', '4', '5', '7.9']

# использование оператора вхождения `in`
# отбираем строки которые имеют вхождение 
# символа точки '.'
>>> f = filter(lambda x: '.' in x, num)
>>> list(f)
# ['2.0', '3.1', '7.9']
```
### Применение обычной функции в качестве фильтра последовательности.
```py
num = list(range(0, 27))

# определяем отдельную функцию которая
# отбирает четные элементы > 18.
def ff(x):
  if x > 18 and not x % 2:
    return True
  else:
    return False

>>> f = filter(ff, num)
>>> list(f)
# [20, 22, 24, 26]
```
_____________
#### Links