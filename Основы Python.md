2021-04-26 19:45
#tag
# Основы Python
[[Итератор Гениратор Py]] | [[Decorators]] | [[Контекстный менеджер]]

[[ООП]] | [[Пространство имён, области видимости]]
[[Файлы в python]]

[[Hash Py]]
[[Типы данных]] | [[Mutable Immutable]] | [[Set Py]]
[[Основы управления памятью в Python]]
Метаклассы магические методы
Алгоритмы (Big_O, сортировки)
Solid dry Kiss

[[Обработка исключений Try Except Finally]] 

#### [Global Interpreter Lock](https://ru.wikipedia.org/wiki/Global_Interpreter_Lock).
Глобальный блокировщик `GIL` следит за тем, чтобы активен был всегда `только один поток`
#### Тернарный оператор
```py
x, y = 25, 50
big = x if x < y else y
```
#### [[Testing]] / Тестирование / [[PyTest]]
В Python есть стандартный модуль [unittest](https://docs.python.org/3/library/unittest.html). Он позволяет объединять тесты в группы, настраивать и автоматизировать их. Дополнение [Mock](https://pypi.python.org/pypi/mock) дает возможность использовать mock-объекты, что облегчает тестирование. 
### Отладка
Python-код можно и нужно отлаживать. Для этого в языке есть специальный интерактивный дебаггер [pdb](http://python-lab.ru/documentation/27/stdlib/pdb.html).
### Расширения на C/C++
Интерпретатор CPython позволяет [внедрять в программы расширения](https://docs.microsoft.com/ru-ru/visualstudio/python/working-with-c-cpp-python-in-visual-studio), которые написаны на C и C++. Разработчик может оптимизировать код и пользоваться библиотеками языка C. Кроме того, можно управлять ресурсами на низком уровне.
_____________
#### Links
[[Python]] [[Собес QA]]