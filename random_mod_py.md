2021-10-27 19:22
[link](https://python-scripts.com/random)
# Random
```python
# получить случайную строку из txt
import random
with open ('myfile.txt', 'r') as file:
    lines = file.readlines()
    print(random.choice(lines))
```
-   [Как использовать модуль random в Python](https://python-scripts.com/random#how-use-random)
-   [Генерация случайных чисел в Python](https://python-scripts.com/random#generator-random-numbers-py)
-   [Выбор случайного элемента из списка](https://python-scripts.com/random#random-element-list)
-   [Python функции модуля random](https://python-scripts.com/random#all-funtions)
-   [Случайное целое число — randint(a, b)](https://python-scripts.com/random#randint)
-   [Генерация случайного целого числа — randrange()](https://python-scripts.com/random#randrange)
-   [Выбор случайного элемента из списка choice()](https://python-scripts.com/random#choice)
-   [Метод sample(population, k)](https://python-scripts.com/random#sample)
-   [Случайные элементы из списка — choices()](https://python-scripts.com/random#choices)
-   [Генератор псевдослучайных чисел — seed()](https://python-scripts.com/random#seed)
-   [Перемешивание данных — shuffle()](https://python-scripts.com/random#shuffle)
-   [Генерации числа с плавающей запятой — uniform()](https://python-scripts.com/random#uniform)
-   [triangular(low, high, mode)](https://python-scripts.com/random#triangular)
-   [Генератор случайной строки в Python](https://python-scripts.com/random#generator-random-string)
-   [Криптографическая зашита генератора случайных данных в Python](https://python-scripts.com/random#crypto-safe)
-   [getstate() и setstate() в генераторе случайных данных Python](https://python-scripts.com/random#get-and-set)
-   [Состояние генератора getstate()](https://python-scripts.com/random#getstate)
-   [Восстанавливает внутреннее состояние генератора — setstate()](https://python-scripts.com/random#setstate)
-   [Зачем нужны функции getstate() и setstate()](https://python-scripts.com/random#for-what-get-set)
-   [Numpy.random — Генератор псевдослучайных чисел](https://python-scripts.com/random#numpy-random)
-   [Генерация случайного n-мерного массива вещественных чисел](https://python-scripts.com/random#n-array-float)
-   [Генерация случайного n-мерного массива целых чисел](https://python-scripts.com/random#n-array-int)
-   [Выбор случайного элемента из массива чисел или последовательности](https://python-scripts.com/random#array-ch-element)
-   [Генерация случайных универсально уникальных ID](https://python-scripts.com/random#generator-uuid)
-   [Игра в кости с использованием модуля random в Python](https://python-scripts.com/random#game-random)

Список **методов модуля random** в Python:

| **Метод** | **Описание** |
|-----------|---------------|
| [seed()](https://python-scripts.com/random#seed) | Инициализация генератора |случайных чисел
[getstate()](https://python-scripts.com/random#getstate) | Возвращает текущее внутренне состояние (state) генератора случайных чисел 
[setstate()](https://python-scripts.com/random#setstate) | Восстанавливает внутреннее состояние (state) генератора случайных чисел
getrandbits() | Возвращает число, которое представляет собой случайные биты
[randrange()](https://python-scripts.com/random#randrange) | Возвращает случайное число в пределах заданного промежутка
[randint()](https://python-scripts.com/random#randint) | Возвращает случайное число в пределах заданного промежутка
[choice()](https://python-scripts.com/random#choice) | Возвращает случайный элемент заданной последовательности
[choices()](https://python-scripts.com/random#choices) | Возвращает список со случайной выборкой из заданной последовательности
[shuffle()](https://python-scripts.com/random#shuffle) | Берет последовательность и возвращает ее в перемешанном состоянии
[sample()](https://python-scripts.com/random#sample) | Возвращает заданную выборку последовательности
random() | Возвращает случайное вещественное число в промежутке от 0 до 1
[uniform()](https://python-scripts.com/random#uniform) | Возвращает случайное вещественное число в указанном промежутке
[triangular()](https://python-scripts.com/random#triangular) | Возвращает случайное вещественное число в промежутке между двумя заданными параметрами. Также можно использовать параметр `mode` для уточнения середины между указанными параметрами
betavariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **Бета-распределении**, которое используется в статистике
expovariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, или же между `0` и `-1`, когда параметр отрицательный. За основу берется **Экспоненциальное распределение**, которое используется в статистике
gammavariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **Гамма-распределении**, которое используется в статистике
gauss() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **Гауссовом распределении**, которое используется в теории вероятности
lognormvariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **Логнормальном распределении**, которое используется в теории вероятности
normalvariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **Нормальном распределении**, которое используется в теории вероятности
vonmisesvariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **распределении фон Мизеса**, которое используется в направленной статистике
paretovariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **распределении Парето**, которое используется в теории вероятности
weibullvariate() | Возвращает случайное вещественное число в промежутке между 0 и 1, основываясь на **распределении Вейбулла**, которое используется в статистике

_____________
#### Links
