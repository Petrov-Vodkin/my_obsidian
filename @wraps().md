2021-12-12 16:14
[docs](https://docs-python.ru/standart-library/modul-functools-python/dekorator-wraps-modulja-functools/)
# @wraps()
Заменить атрибуты декоратора на атрибуты исходной функции.
```py
from functools import wraps

@wraps(wrapped, assigned=WRAPPER_ASSIGNMENTS, 
       updated=WRAPPER_UPDATES)
```
#### Параметры:
-   `wrapped` - исходная [функция](https://docs-python.ru/tutorial/opredelenie-funktsij-python/ "Функции в Python, определение функций."),
-   `assigned` - какие атрибуты исходной функции присваиваются [декоратору](https://docs-python.ru/tutorial/dekoratory-python/ "Создание и использование декораторов в Python."),
-   `updated` - какие атрибуты декоратора обновляются из исходной функции.
```py
from functools import wraps
def my_decorator(f):
    @wraps(f)
    def wrapper(*args, **kwds):
        print('Calling decorated function')
        return f(*args, **kwds)
    return wrapper

@my_decorator
def example():
    """Docstring"""
    print('Called example function')

example()			# Calling decorated function # Called example function
example.__name__	# 'example'
example.__doc__		# 'Docstring'
```
_____________
#### Links
[[]]