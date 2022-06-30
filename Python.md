2020-12-03 16:47
#питон #глобальные #переменные [python](https://www.python.org/)
# Python
[[Основы Python]]  |  [[Встроенные функции Py]]
[[Библиотека Python]] 
[[Асинхронность Py]]
[[Python WEB]]
[[VenvPy]]
[[Сниппеты Python]] 
[[Selenium сниппеты]]


Утилита `py` - это лаунчер, который с помощью ключей командной строки позволяет запускать нужную версию Python. Справка по этой утилите:

```bash
py --list       # возможные версии py (VersionNum)

C:\Users\User>py --help
py [launcher-args] [python-args] script [script-args]

Launcher arguments:
-2     # запустить Python 2 самой новой версии из "ветки" 2) Python 2.7, нужно в командной строке указать `py -2.7`
-3     # Launch the latest Python 3.x version
-X.Y   # Launch the specified Python version
     The above all default to 64 bit if a matching 64 bit python is present.
-X.Y-32# Launch the specified 32bit Python version
-X-32  # Launch the latest 32bit Python X version
-X.Y-64# Launch the specified 64bit Python version
-X-64  # Launch the latest 64bit Python X version
-0  --list       : List the available pythons
-0p --list-paths : List with paths
		'pip'
# добавь в параметры среды\сист-е переменные PATH
C:\Users\Di13\AppData\Local\Programs\Python\Python39\Scripts 
```

Также эта утилита умеет брать версию Python из [shebang](https://ru.wikipedia.org/wiki/%D0%A8%D0%B5%D0%B1%D0%B0%D0%BD%D0%B3_(Unix)) строки скрипта: если в начале скрипта написать `#!python2.7`, и запустить скрипт с помощью команды  
`py имя_скрипта.py`, то скрипт будет запущен с помощью Python версии 2.7 (или другой версии, которую вы укажете).

#### Глобальная переменная в функции***(пр. - для изменения этой переменной)
```py
primer = 1 или "" or () or [] и тд # инициализирум переменную выше функции 
def name_funck():
	global primer #	инициализирум переменную в функции
```
#### Кодировка
```python
# encode
import sys
import codecs

sys.stdout = codecs.getwriter("utf-8")(sys.stdout.detach())
```

#### Links

#### [[Ресурсы]]
https://python-scripts.com/
[docs.python.org](https://docs.python.org/3/ 'Tutorial')
[Запуск разных версий ](https://ru.stackoverflow.com/questions/1136301/%d0%9f%d0%be%d0%bc%d0%b5%d0%bd%d1%8f%d1%82%d1%8c-%d0%b2%d0%b5%d1%80%d1%81%d0%b8%d1%8e-python-%d0%b2-windows-10#1136316)