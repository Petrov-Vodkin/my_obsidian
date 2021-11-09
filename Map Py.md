2021-11-09 08:08
#tag
# Map Py
Применяет определенную функцию к каждому элементу в последовательности.

```py
map(function, iterable, ...)
```
#### Параметры:
-   [`function`](https://docs-python.ru/tutorial/opredelenie-funktsij-python/ "Функции в Python, определение функций.") - пользовательская функция, вызывается для каждого элемента,
-   `iterable` - последовательность или объект, поддерживающий итерирование.
#### Возвращаемое значение:

-   `map object` - объект итератора.

#### Описание:

Функция [`map()`](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-map/ "Функция map() в Python, обработка последовательности без цикла.") выполняет пользовательскую [функцию](https://docs-python.ru/tutorial/opredelenie-funktsij-python/ "Функции в Python, определение функций.") `function` для каждого элемента [последовательности](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tipy-posledovatelnostej/ "Типы последовательностей"), коллекции или [итератора](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-iterator-iterator/ "Тип данных Iterator (итератор).") `iterable`. Каждый элемент `iterable` отправляется в функцию `function` в качестве аргумента.

Если в `map()` передаётся несколько `iterable`, то пользовательская функция `function` должна принимать количество аргументов, соответствующее количеству переданных последовательностей, при этом `function` будет применяться к элементам из всех итераций параллельно.
```py
# функция должна принимать столько
# аргументов, сколько последовательностей
# передается в функцию map()
def plus(a, b, c):
  return a + b +c

# функция 'plus' применяется к элементам 
# из всех последовательностей параллельно
>>> x = map(plus, [1, 2], [3, 4], [5, 6])
>>> list(x)
# [9, 12]
```
При использовании нескольких последовательностей, функция `map()` останавливается, когда исчерпывается самая короткая итерация.
```py
def create_tuple(a, b):
    return a, b

# функция `map()` останавливается, когда 
# заканчивается самая короткая последовательность
>>> x = map(create_tuple, ['a', 'b'], [3, 4, 5])
>>> print(dict(x))
# {'a': 3, 'b': 4}
```
Можно так же использовать любую [встроенную функцию](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/ "Встроенные функции Python.") с функцией `map()` при условии, что функция принимает аргумент и возвращает значение.
```py
>>> x = [1, 2, 3]
>>> y = [4, 5, 6, 7]
# вычисление при помощи встроенной функции 'pow()' 
# 'x' в степени 'y' для каждого элемента 2-х списков
>>> list(map(pow, x, y))
# [1, 32, 729]
```
Для случаев, когда входные данные функции уже организованы в кортежи аргументов, смотрите [функцию `itertools.starmap()`](https://docs-python.ru/standart-library/modul-itertools-python/funktsija-starmap-modulja-itertools/ "Функция starmap() модуля itertools в Python.").

Преимуществ использования `map()`:

-   Так как [функция `map()`](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-map/ "Функция map() в Python, обработка последовательности без цикла.") написана на языке C и хорошо оптимизирована, ее внутренний цикл более эффективный, чем обычный [цикл `for`](https://docs-python.ru/tutorial/tsikly-upravlenie-vetvleniem-python/tsikl-for-in/ "Введение в цикл for/in в Python.") в Python.
-   Низкое потребление памяти. С помощью цикла `for .. in:` программе необходимо хранить в памяти системы весь [список](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-list-spisok/ "Тип данных list, список") элементов последовательности над которым производятся какие-то действия внутри цикла. При помощи функции `map()` элементы последовательности извлекаются по запросу, следовательно при каждом внутреннем цикле `map()` в памяти системы находится и обрабатывается только один элемент последовательности.

### Примеры обработки последовательностей без циклов.

-   [Подсчет количества символов в каждом элементе кортежа](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-map/#len);
-   [Создание словаря из двух списков](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-map/#dict);
-   [Удаление пунктуации в тексте](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-map/#punct).
```py
#### Подсчет количества символов в каждом элементе кортежа:

>>> x = map(len, ('apple', 'banana', 'cherry'))
>>> list(x)
# [5, 6, 6]

#### Создание словаря из двух списков.

>>> x = map(lambda *args: args, [1, 2], [3, 4])
>>> dict(x)
# {1: 3, 2: 4}

#### Удаление пунктуации в тексте при помощи `map()`.

>>> import re
>>> def clean(word):
...        return re.sub(r"[`!?.:;,'\"()-]", "", word.strip())

>>> text = """С помощью цикла `for .. in:` программе 
необходимо хранить в памяти системы весь (список)! """
>>> word = text.split()
>>> word = map(clean, word)
>>> text = ' '.join(word)
>>> text
# 'С помощью цикла for  in программе необходимо 
# хранить в памяти системы весь список'
```
_____________
#### Links
https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-map/