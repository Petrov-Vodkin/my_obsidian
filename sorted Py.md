2021-11-09 14:21
[doc_ru](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-sorted/)
# sorted Py
Выполняет сортировку последовательности по возростанию/убыванию.

#### Синтаксис:
```py
sorted(iterable, *, key=None, reverse=False)
```

sorted() является гарантированно стабильной: когда несколько элементов последовательности имеют равные значения, `их первоначальный порядок сохраняется`. 
#### Параметры:
-   `iterable` - объект, поддерживающий итерирование
-   `key=None` - пользовательская функция, которая применяется к каждому элементу последовательности
-   `reverse=False` - порядок сортировки

#### Описание:
[`sorted()`](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-sorted/ "Функция sorted() в Python, выполняет сортировку.") вернет новый отсортированный [список] из итерируемых элементов. 

аргумент `key` принимает функцию, например `key=str.upper`
```py
				# случай с пользовательской func
numb = (22, 10, 5, 34, 29)
sorted(numb, key=lambda x: x%5)	# [10, 5, 22, 34, 29]
Мы использовали лямбда-функцию для простоты, но вы можете использовать и традиционный метод определения функции.
```
Аргумент `reverse=False` имеет [логическое](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/bool-logicheskij-tip-dannyh/ "bool, логический тип данных.") значение. Если установлено значение `True`, то элементы списка сортируются в обратной последовательности (по убыванию).

Используйте [`functools.cmp_to_key()`](https://docs-python.ru/standart-library/modul-functools-python/funktsija-cmp-to-key-modulja-functools/ "Функция cmp_to_key() модуля functools в Python.") для преобразования функции, использующей cmp (старый стиль) в использующую key (новый стиль).


### Примеры различных способов сортировки последовательностей.

Сортировка слов в предложении без учета регистра:

```py
line = 'This is a test string from Andrew'
x = sorted(line.split(), key=str.lower)
print(x)
# ['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']

Сортировка сложных объектов с использованием индексов в качестве ключей `key`:

student = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
    ]

# Сортируем по возрасту student[2]
x = sorted(student, key=lambda student: student[2])
print(x)
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]

Тот же метод работает для объектов с именованными атрибутами.

class Student:
    def __init__(self, name, grade, age):
        self.name = name
        self.grade = grade
        self.age = age
    def __repr__(self):
        return repr((self.name, self.grade, self.age))

student = [
    Student('john', 'A', 15),
    Student('jane', 'B', 12),
    Student('dave', 'B', 10),
    ]

x = sorted(student, key=lambda student: student.age)
print(x)
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]

Сортировка по убыванию:

student = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
    ]

# Сортируем по убыванию возраста student[2]
x = sorted(student, key=lambda i: i[2], reverse=True)
print(x)
# [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]

Стабильность сортировки и сложные сортировки:

data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]
x = sorted(data, key=lambda data: data[0])
print(x)
# [('blue', 1), ('blue', 2), ('red', 1), ('red', 2)]
```
Обратите внимание, как две записи ('blue', 1), ('blue', 2) для синего цвета сохраняют свой первоначальный порядок.

Это замечательное свойство позволяет создавать сложные сортировки за несколько проходов по нескольким ключам. Например, чтобы отсортировать данные учащиеся по возрастанию успеваемости, а затем по убыванию возраста. Успеваемость будет первым ключом, возраст вторым.

```py
student = [
    ('john', 15, 4.1),
    ('jane', 12, 4.9),
    ('dave', 10, 3.9),
    ('kate', 11, 4.1),
    ]

# По средней оценке
x = sorted(student, key=lambda num: num[2])
# По убыванию возраста
y = sorted(x, key=lambda age: age[1], reverse=True)
print(y)
# [('john', 15, 4.1), ('jane', 12, 4.9), ('kate', 11, 4.1), ('dave', 10, 3.9)]
```
А еще, для сортировок по нескольким ключам, удобнее использовать [модуль `operator`](https://docs-python.ru/standart-library/modul-operator-python/ "Модуль operator, интерфейс встроенных операторов Python."). Функции модуля `operator` допускают несколько уровней сортировки. Например, как в предыдущем примере успеваемость будет первым ключом, возраст вторым.  
Только сортировать будем все по возрастанию:
```py
from operator import itemgetter

x = sorted(student, key=itemgetter(2,1))
print(x)
# [('dave', 10, 3.9), ('kate', 11, 4.1), ('john', 15, 4.1), ('jane', 12, 4.9)]
```
_____________
#### Links
[[Встроенные функции Py]]
