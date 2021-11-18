2021-11-17 17:19
#tag
# Str Py
### [Операции с текстовыми строками str в Python](https://docs-python.ru/tutorial/operatsii-tekstovymi-strokami-str-python/)
#### str.endswith()
совпадение с концом строки `str.endswith(suffix[, start[, end]])`
-   `suffix` - [объект поддерживающий итерацию](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-iterator-iterator/ "Тип данных Iterator (итератор).") (кортеж, символ или подстрока).
-   `start` - [`int`](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-int-tselye-chisla/ "Тип данных int, целые числа"), индекс начала поиска, по умолчанию `0`, необязательно.
-   `end` - `int`, индекс конца поиска, по умолчанию [`len(str)`](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-len/ "Функция len() в Python, считает количество элементов."), необязательно.

#### Возвращаемое значение:

-   [`bool`](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/bool-logicheskij-tip-dannyh/ "bool, логический тип данных."), `True`, если суффикс `suffix` совпал.

#### Описание:

Метод [`str.endswith()`](https://docs-python.ru/tutorial/operatsii-tekstovymi-strokami-str-python/metod-str-endswith/ "Метод str.endswith() в Python, совпадение с концом строки.") возвращает `True`, если строка `str` **заканчивается указанным суффиксом `suffix`**, в противном случае возвращает `False`.

Ограничивать поиск окончания строки можно необязательными индексами `start` и `end`. В этом случае суффиксом `suffix` будет искаться в конце среза.

-   Суффикс `suffix` также может быть [кортежем](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-tuple-kortezh/ "Тип данных tuple, кортеж") суффиксов для поиска.
-   Необязательные аргументы `start` и `end` интерпретируются как обозначения [среза строки](https://docs-python.ru/tutorial/obschie-operatsii-posledovatelnostjami-list-tuple-str-python/izvlechenie-sreza-sequence-posledovatelnosti/ "Получение среза sequence[i:j] в Python.") и передаются как [позиционные аргументы](https://docs-python.ru/tutorial/opredelenie-funktsij-python/varianty-peredachi-argumentov-funktsiju/ "Варианты передачи аргументов в функцию Python.")
-   При вызове без аргументов бросает исключение `TypeError` (требуется как минимум `1` аргумент, передано `0`).

Для поиска строк с требуемым префиксом используйте метод строки [`str.startswith()`](https://docs-python.ru/tutorial/operatsii-tekstovymi-strokami-str-python/metod-str-startswith/ "Метод str.startswith() в Python, совпадение с началом строки.").

### Примеры поиска строк с заданным суффиксом.
```py
x = 'заканчивается указанным суффиксом suffix'
x.endswith('suffix')		# True
x.endswith('иксом')			# False
x.endswith('иксом',0 ,-7)	# True

# Есть список строк
x = ['возвращает True', 'если строка str', 'заканчивается указанным', 'суффиксом suffix']

# Нужны строки, заканчивающиеся на суффиксы
suffix = ('ue', 'str', 'fix')

for item in x:
    print('OK =>', item) if item.endswith(suffix) else print('NO =>', item)

# OK => возвращает True
# OK => если строка str
# NO => заканчивается указанным
# OK => суффиксом suffix

```
_____________
#### Links
