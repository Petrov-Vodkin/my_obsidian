2021-11-17 17:19
#tag
# Str Py
### Методы
```py
print("abc" in "abcba")  
print("abce" in "abcba")  
						# индекс первого вхождения или -1  
print("cabcd".find("abc", 1))  
print("cabcd"[1:].find("abc"))  
print(str.find.__doc__)		# вывод док-и str
print(s.rfind("aba"))  		# возвращает индекс вхождения в строку с права

					# индекс первого вхождения или ValueError  
print("cabcd".index("abc"))  
print("cabcd".index("aec"))  
					# начало строки с ... (True || False)  
s = "The whale in black fled across the desert, and the gunslinger followed"  
print(s.startswith(("The woman", "The dog", "The man in black")))  
print(s.startswith.__doc__)  
  					# конец строки
s = "image.png"  
print(s.endswith(".png"))  
  					# кол-во вхождений
s = "abacaba"  
print(s.count("aba"))  # 2
  					# изменение РеГистРа
s = "The man in black fled across the desert, and the gunslinger followed"  
print(s.lower())  
print(s.upper())  
					# найти и ЗАМЕНИТЬ  
s = "1,2,3,4"  
print(s.replace(",", ", ", 2))  
					# РАЗДЕЛИТЬ по символу  
s = "1\t\t 2  3       4       "print(s.split())  
print(s.split.__doc__)  
					# объеденить коллекцию в строку  
numbers = map(str, [1, 2, 3, 4, 5])  
print(repr(" ".join(numbers)))  
  					# Форматрование
capital = 'London is the capital of Great Britain'  
template = '{} is the capital of {}'  
print(template.format("London", "Great Britain"))  
print(template.format("Vaduz", "Liechtenstein"))  
  
template = '{capital} is the capital of {country}'  
print(template.format(capital="London", country="Great Britain"))  
print(template.format(country="Liechtenstein", capital="Vaduz"))  
  
  
import requests  
template = "Response from {0.url} with code {0.status_code}"  
  
res = requests.get("https://docs.python.org/3.5/")  
print(template.format(res))  
  
res = requests.get("https://docs.python.org/3.5/random")  
print(template.format(res))  
  
from random import random  
x = random()  
print(x)  
print("{:.3}".format(x))
```
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
[[Регулярные выражения py]]
