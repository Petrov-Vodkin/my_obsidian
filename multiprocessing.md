2021-02-04 15:05
[doc_en](https://docs.python.org/3/library/multiprocessing.html)  [doc_ru](https://pythonim.ru/moduli/multiprocessing-python)
# Multiprocessing
#### [[AIOHTTP]] | [[Паралельный запуск тестов pytest]] | [[async test patterns for Pytest]]
`start()` 	Метод start() используется для запуска процесса.
`join() ([timeout])` 	Метод join() используется для блокировки процесса до тех пор, пока не завершится процесс, каким метод join() вызван. Timeout – необязательный аргумент.

##### Узнать кол-во процессов в системе
```python
from multiprocessing import cpu_count
print( 'число процессоров = {}'.format( cpu_count() ) )
```
## Pool
```py
# so EASY ))) юзал с request, НО работает как ПОТОКИ
from multiprocessing.dummy import Pool as ThreadPool 

pool = ThreadPool(10) # say
all = pool.map(check_vulnerability, packages)
```
## Process[](https://teletype.in/@python_academy/T5DXZot4I)
___!!!!!___ аргументы  в функции `Process(target=func, args=(number,))` это __`всегда кортеж`__, даже если там всего один элемент.
```python
import os
from multiprocessing import Process


def func(number):
    proc = os.getpid()
    print(f'{number} squared to {number ** 2} by process id: {proc}')
# Теперь наконец создадим функцию, для создания 5 процессов для 5 целых чисел, и посмотрим что получилось.
def main():
    procs = []
	# можно через генератор ))  тогда ннада в  РАЗНЫХ циклах proc.start() и  proc.join()
	# procs_v1 = (Process(target=func, args=(number,)) for number in numbers)
    numbers = [1, 2, 3, 4, 5]

    for number in numbers:
        proc = Process(target=func, args=(number,))	#  args=(number,)  !!! КОРТЕЖ !!!
        procs.append(proc)
        proc.start()
		
    for proc in procs:
        proc.join()
		
if __name__ == '__main__': 
	main()		
```
### Класс Pool
#### v2
Попробуем применить метод `requests.get()` к некоторому списку сайтов. Последовательное выполнение задачи отнимет много времени, но что если сделать это параллельно?
```py
import requests
from multiprocessing import Pool


def main(processes):
    urls = ['http://www.python.org', 'http://www.python.org/about/', 'http://www.python.org/doc/', 'http://www.python.org/download/', 'http://www.python.org/getit/', 'http://www.python.org/community/',  'https://wiki.python.org/moin/', 'http://planet.python.org/', 'http://www.python.org/psf/']
    # создадим пул процессов
    with Pool(processes=processes) as pool:
        # получим результат с помощью функции map
        results = pool.map(requests.get, urls)

# Теперь сравним скорости обработки при различном размере пула.
if __name__ == '__main__':
    main(processes=13)	# -- 13 Pool -- 2.6185 sec
    main(processes=8)	# -- 8 Pool --	2.7000 sec
    main(processes=4)	# -- 4 Pool --	3.0071 sec
    main(processes=1)	# -- Single -- 	7.3520 sec
```
скорость выполнения таких процессов сильно зависит от скорости и качества интернет соединения. Кроме того, логически более правильно здесь было бы использовать `async`, а не `multiprocessing`. Однако даже в таком примере виде прирост скорости.

#### v1
Класс Pool используется для показа пула рабочих процессов. Он включает в себя методы, которые позволяют вам разгружать задачи к рабочим процессам. Давайте посмотрим на простейший пример:

```Python
from multiprocessing import Pool
 
 
def doubler(number):
    return number * 2
 
 
if __name__ == '__main__':
    numbers = [5, 10, 20]
    pool = Pool(processes=3)
    print(pool.map(doubler, numbers))
```

Здесь мы создали экземпляр **Pool** и указали ему создать три рабочих процесса. Далее мы используем метод **map** для отображения функции для каждого процесса. Наконец мы выводим результат, что в нашем случае является списком: [10, 20, 40]. Вы также можете получить результат вашего процесса в пуле, используя метод **apply_async**:

```Python
from multiprocessing import Pool
 
 
def doubler(number):
    return number * 2
 
 
if __name__ == '__main__':
    pool = Pool(processes=3)
    result = pool.apply_async(doubler, (25,))
    print(result.get(timeout=1))
```

Так мы можем запросить результат процесса. В этом суть работы функции **get**. Она пытается получить наши результаты. Обратите внимание на то, что мы также настроили обратный отсчет, на тот случай, если что-нибудь произойдет с вызываемой нами функцией. Мы не хотим, чтобы она была заблокирована. 

**Общая память**

Данные могут храниться в отображении общей памяти с использованием [`Value`](https://digitology.tech/docs/python_3/library/multiprocessing.html#multiprocessing.Value "multiprocessing.Value") или [`Array`](https://digitology.tech/docs/python_3/library/multiprocessing.html#multiprocessing.Array "multiprocessing.Array"). Например, следующий код
```py
from multiprocessing import Process, Value, Array

def f(n, a):
    n.value = 3.1415927
    for i in range(len(a)):
        a[i] = -a[i]

if __name__ == '__main__':
    num = Value('d', 0.0)
    arr = Array('i', range(10))

    p = Process(target=f, args=(num, arr))
    p.start()
    p.join()

    print(num.value)	# 3.1415927
    print(arr[:])		# [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]

```
Аргументы `'d'` и `'i'`, используемые при создании `num` и `arr`, являются кодами типа того типа, который используется модулем [`array`](https://digitology.tech/docs/python_3/library/array.html#module-array "array: Пространственно-эффективные массивы типизированных числовых значений."): `'d'` указывает на число с плавающей запятой двойной точности, а `'i'` указывает на целое число со знаком. Эти общие объекты будут технологическими и поточно-ориентированными.

Для большей гибкости в использовании разделяемой памяти можно использовать модуль [`multiprocessing.sharedctypes`](https://digitology.tech/docs/python_3/library/multiprocessing.html#module-multiprocessing.sharedctypes "multiprocessing.sharedctypes: Аллоцировать объекты ctypes из общей памяти."), который поддерживает создание произвольных объектов ctypes, выделенных из разделяемой памяти

### Прокси-объекты

Прокси-объекты называются **`общими объектами`**, которые находятся в другом процессе. Этот объект также называется прокси. У нескольких прокси-объектов может быть похожий референт. Прокси-объект состоит из различных методов, которые используются для вызова соответствующих методов его референта. Ниже приведен пример прокси-объектов.
```py
from multiprocessing import Manager 
manager = Manager() 
l = manager.list([i*i for i in range(10)]) 
print(l) 		# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
print(repr(l)) 	# <ListProxy object, typeid 'list' at 0x7f063621ea10>
print(l[4]) 	# 16
print(l[2:5]) 	# [4, 9, 16]

```
Прокси-объекты можно выбирать, поэтому мы можем передавать их между процессами. Эти объекты также используются для контроля над синхронизацией.
Источник: https://pythonpip.ru/osnovy/mnogoprotsessornost-python


_____________
#### Links
[[Python]]
https://pythonpip.ru/osnovy/mnogoprotsessornost-python
https://digitology.tech/docs/python_3/library/multiprocessing.html