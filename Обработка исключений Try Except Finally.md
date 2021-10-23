2021-09-25 09:47
#tag
# Обработка исключений Try Except Finally
### 1\. Обработка исключений Python

В этом материале речь пойдет о блоках `try/except`, `finally` и `raise`. Вместе с тем будет рассмотрено, как создавать собственные исключения в Python.

### 2\. Обработка исключений в Python

Рассмотрим разные типы исключений в Python, которые появляются при срабатывании исключения в коде Python.

### 3\. Блоки try/except

Если код может привести к исключению, его лучше заключить в блок `try`. Рассмотрим на примере.

```py
try:
    for i in range(3):
        print(3/i)
except:
    print("Деление на 0")
    print("Исключение было обработано")
```

Программа вывела сообщение, потому что было обработано исключение.

Следом идет блок `except`. Если не определить тип исключения, то он будет перехватывать любые. Другими словами, это общий обработчик исключений.

Если код в блоке `try` приводит к исключению, интерпретатор ищет блок `except`, который указан следом. Оставшаяся часть кода в `try` исполнена не будет.

Исключения Python особенно полезны, если программа работает с вводом пользователя, ведь никогда нельзя знать, что он может ввести.

#### a. Несколько except в Python

У одного блока `try` может быть несколько блоков `except`. Рассмотрим примеры с несколькими вариантами обработки.

```py
a, b = 1, 0
try:
    print(a/b)
    print("Это не будет напечатано")
    print('10'+10)
except TypeError:
    print("Вы сложили значения несовместимых типов")
except ZeroDivisionError:
    print("Деление на 0")
```

Когда интерпретатор обнаруживает исключение, он проверяет блоки `except` соответствующего блока `try`. В них может быть объявлено, какие типы исключений они обрабатывают. Если интерпретатор находит соответствующее исключение, он исполняет этот блок `except`.

В первом примере первая инструкция приводит к `ZeroDivisionError`. Эта ошибка обрабатывается в блоке `except`, но инструкции в `try` после первой не исполняются. Так происходит из-за того, что после первого исключения дальнейшие инструкции просто пропускаются. И если подходящий или общий блоки `except` не удается найти, исключение не обрабатывается. В таком случае оставшаяся часть программы не будет запущена. Но если обработать исключение, то код после блоков `except` и `finally` исполнится. Попробуем.

```py
a, b = 1, 0
try:
   print(a/b)
except:
   print("Вы не можете разделить на 0")
print("Будет ли это напечатано?")

# Рассмотрим вывод:
Вы не можете разделить на 0
Будет ли это напечатано?
```

#### b. Несколько исключений в одном except

Можно использовать один блок `except` для обработки нескольких исключений. Для этого используются скобки. Без них интерпретатор вернет [синтаксическую ошибку](https://pythonru.com/osnovy/7-python-dlja-data-science-osnovy-sintaksisa-python).

```py
try:
    print('10'+10)
    print(1/0)
except (TypeError,ZeroDivisionError):
    print("Неверный ввод")
# Неверный ввод
```

#### c. Общий except после всех блоков except

В конце концов, завершить все отдельные блоки `except` можно одним общим. Он используется для обработки всех исключений, которые не были перехвачены отдельными `except`.

```py
try:
    print('1'+1)
    print(sum)
    print(1/0)
except NameError:
    print("sum не существует")
except ZeroDivisionError:
    print("Вы не можете разделить на 0")
except:
    print("Что-то пошло не так...")
# Что-то пошло не так...
```

Здесь первая инструкция блока пытается осуществить операцию конкатенации [строки python](https://pythonru.com/osnovy/stroki-python) с числом. Это приводит к ошибке `TypeError`. Как только интерпретатор сталкивается с этой проблемой, он проверяет соответствующий блок `except`, который ее обработает.

Отдельную инструкцию нельзя разместить между блоками `try` и `except`.

```py
try:
    print("1")
print("2")
except:
    print("3")
```
Это приведет к синтаксической ошибке.
Но может быть только один общий или блок по умолчанию типа `except`. Следующий код вызовет ошибку `«default 'except:' must be last»`:

```py
try:
    print(1/0)
except:
    raise
except:
    print("Исключение поймано")
finally:
    print("Хорошо")
print("Пока")
```

### 4\. Блок finally в Python

После последнего блока `except` можно добавить блок `finally`. Он исполняет инструкции при любых условиях.

```py
try:
    print(1/0)
except ValueError:
    print("Это ошибка значения")
finally:
    print("Это будет напечатано в любом случае.")
# Это будет напечатано в любом случае.

Traceback (most recent call last):  
  File “<pyshell#113>”, line 2, in <module>  
    print(1/0)
ZeroDivisionError: division by zero
```

Стоит обратить внимание, что сообщение с ошибкой выводится после исполнения блока `finally`. Почему же тогда просто не использовать `print`? Но как видно по последнему примеру, блок `finally` запускается даже в том случае, если перехватить исключение не удается.

А что будет, если исключение перехватывается в `except`?

```py
try:
    print(1/0)
except ZeroDivisionError:
    print(2/0)
finally:
    print("Ничего не происходит")
# Ничего не происходит

Traceback (most recent call last):
  File "<pyshell#1>", line 2, in <module>
    print(1/0)
ZeroDivisionError: division by zero

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<pyshell#1>", line 4, in <module>
    print(2/0)
ZeroDivisionError: division by zero
```

Как видите, код в блоке `finally` исполняется в любом случае.

### 5\. Ключевое слово raise в Python

Иногда нужно будет разбираться с проблемами с помощью вызова исключения. Обычная инструкция `print` тут не сработает.

```py
raise ZeroDivisionError
```

```
Traceback (most recent call last):
  File "<pyshell#2>", line 1, in <module>
    raise ZeroDivisionError
ZeroDivisionError
```

Разберемся на примере операции деления:

```py
a,b=int(input()),int(input())  
if b==0:
    raise ZeroDivisionError
```

```
Traceback (most recent call last):
  File "<pyshell#2>", line 3, in <module>
    raise ZeroDivisionError
ZeroDivisionError
```

Здесь ввод пользователя в переменные `a` и `b` конвертируется в целые числа. Затем проверяется, равна ли `b` нулю. Если да, то вызывается `ZeroDivisionError`.

Что будет, если то же самое добавить в блоки try-except? Добавим следующее в код. Если запустить его, ввести 1 и 0, будет следующий вывод:

```py
a,b=int(input()),int(input())
try:
    if b==0:
        raise ZeroDivisionError
except:
   print("Деление на 0")
print("Будет ли это напечатано?")
```

```
1
0
Деление на 0
Будет ли это напечатано?
```

Рассмотрим еще несколько примеров, прежде чем двигаться дальше:

```py
raise KeyError
```

```
Traceback (most recent call last):
  File “<pyshell#180>”, line 1, in <module>
    raise KeyError
KeyError
```

#### a. Raise без определенного исключения в Python

Можно использовать ключевое слово `raise` и не указывая, какое исключение вызвать. Оно вызовет исключение, которое произошло. Поэтому его можно использовать только в блоке `except`.

```py
try:
    print('1'+1)
except:
    raise
```

```
Traceback (most recent call last):
  File “<pyshell#152>”, line 2, in <module>
    print(‘1’+1)
TypeError: must be str, not int
```

#### b. Raise с аргументом в Python

Также можно указать аргумент к определенному исключению в `raise`. Делается это с помощью дополнительных деталей исключения.

```py
raise ValueError("Несоответствующее значение")
```

```
Traceback (most recent call last):
  File "<pyshell#3>", line 1, in <module>
    raise ValueError("Несоответствующее значение")
ValueError: Несоответствующее значение
```

### 6\. assert в Python

Утверждение (assert) — это санитарная проверка для вашего циничного, параноидального «Я». Оно принимает инструкцию в качестве аргумента и вызывает исключение Python, если возвращается значение `False`. В противном случае выполняет операцию No-operation (NOP).

```py
assert(True)

```

Если бы инструкция была `False`?

```py
assert(1==0)
```

```
Traceback (most recent call last):
  File “<pyshell#157>”, line 1, in <module>
    assert(1==0)
AssertionError
```

Возьмем другой пример:

```py
try:
    print(1)
    assert 2+2==4
    print(2)
    assert 1+2==4
    print(3)
except:
    print("assert False.")
    raise
finally:
    print("Хорошо")
print("Пока")
```

Вывод следующий:

```
1
2
assert False.
Хорошо
Traceback (most recent call last):
  File “<pyshell#157>”, line 5, in <module>
    assert 1+2==4
AssertionError
```

Утверждения можно использовать для проверки валидности ввода и вывода в функции.

#### a. Второй аргумент для assert

Можно предоставить второй аргумент, чтобы дать дополнительную информацию о проблеме.

```py
assert False,"Это проблема"
```

```
Traceback (most recent call last):
  File “<pyshell#173>”, line 1, in <module>
    assert False,”Это проблема”
AssertionError: Это проблема
```

### 7\. Объявление собственных исключений Python

Наконец, рассмотрим процесс создания собственных исключений. Для этого создадим новый класс из класса `Exception`. Потом его можно будет вызывать как любой другой тип исключения.

```py
class MyError(Exception):
    print("Это проблема")

raise MyError("ошибка MyError")
```

```
Traceback (most recent call last):
  File “<pyshell#179>”, line 1, in <module>
    raise MyError(“ошибка MyError”)
MyError: ошибка MyError
```

Вот и все, что касается обработки исключений в Python.

### 8\. Вывод: обработка исключений Python

Благодаря этой статье вы сможете обеспечить дополнительную безопасность своему коду. Все благодаря возможности обработки исключений Python, их вызова и создания собственных.

От класса **Exception** наследуется больше десятка различных ошибок, которые может обработать программист. Вот лишь некоторые из них:

-   **IOError** – ошибка ввода-вывода, например, "файл не найден".
-   **ImportError** – ошибка импорта модуля.
-   **IndexError** – обращение к несуществующему индексу последовательности.
-   **OSError** – ошибка системы.
-   **SyntaxError** – синтаксическая ошибка.
-   **TypeError** – ошибка типа данных, например, функция вызывается с неподходящим по типу аргументом.
-   **ZeroDivisionError** – деление на ноль.
_____________
###### Links
https://pythonru.com/osnovy/obrabotka-iskljuchenij-python-blok-try-except-blok-finally