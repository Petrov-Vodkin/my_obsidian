2021-02-04 15:05
#потоки #процессы #колвопроцессов #многопроцессор [doc_en](https://docs.python.org/3/library/multiprocessing.html)
# Multiprocessing
[[AIOHTTP]] | [[Паралельный запуск тестов pytest]] | [[async test patterns for Pytest]]
`start()` 	Метод start() используется для запуска процесса.
`join() ([timeout])` 	Метод join() используется для блокировки процесса до тех пор, пока не завершится процесс, каким метод join() вызван. Timeout – необязательный аргумент.

##### Узнать кол-во процессов в системе
```python
from multiprocessing import cpu_count
print( 'число процессоров = {}'.format( cpu_count() ) )
```
## РАБОТАЕТ(потоки)
```py
# so EASY ))) юзал с request, НО работает как ПОТОКИ
from multiprocessing.dummy import Pool as ThreadPool 

pool = ThreadPool(10) # say
all = pool.map(check_vulnerability, packages)
```
_____________________________________________________
[Для этого примера](http://python-3.ru/page/multiprocessing)  мы импортируем **Process** и создаем функцию `doubler`. Внутри функции, мы дублируем число, которое мы ей передали. Мы также используем [модуль os](https://python-scripts.com/import-os-example), чтобы получить **ID нынешнего процесса**. Это скажет нам, какой именно процесс вызывает функцию. Далее, в нижнем блоке кода, мы создаем несколько Процессов и начинаем их.
Самый последний цикл только вызывает метод `join()` для каждого из процессов, что говорит Python подождать, пока процесс завершится. Если вам нужно остановить процесс, вы можете вызвать метод `terminate()`. Когда вы запустите этот код, вы получите выдачу, на подобие этой:
```Python
import os
from multiprocessing import Process
 
 
def doubler(number):
    """
    Функция умножитель на два
    """
    result = number * 2
    proc = os.getpid()
    print('{0} doubled to {1} by process id: {2}'.format(
        number, result, proc))
 
 
if __name__ == '__main__':
    numbers = [5, 10, 15, 20, 25]
    procs = []
    
    for index, number in enumerate(numbers):
        proc = Process(target=doubler, args=(number,))
        procs.append(proc)
        proc.start()
    
    for proc in procs:
        proc.join()
```
### Класс Pool
#### v2
Попробуем применить метод `requests.get()` к некоторому списку сайтов. Последовательное выполнение задачи отнимет много времени, но что если сделать это параллельно?
```py
import requests
from multiprocessing import Pool


def main(processes):
    urls = [
        'http://www.python.org',
        'http://www.python.org/about/',
        'http://www.onlamp.com/pub/a/python/2003/04/17/metaclasses.html',
        'http://www.python.org/doc/',
        'http://www.python.org/download/',
        'http://www.python.org/getit/',
        'http://www.python.org/community/',
        'https://wiki.python.org/moin/',
        'http://planet.python.org/',
        'https://wiki.python.org/moin/LocalUserGroups',
        'http://www.python.org/psf/',
        'http://docs.python.org/devguide/',
        'http://www.python.org/community/awards/'
    ]
    
    # создадим пул работников
    with Pool(processes=processes) as pool:
        # получим результат с помощью функции map
        results = pool.map(requests.get, urls)

# Теперь сравним скорости обработки при различном размере пула.
if __name__ == '__main__':
    # -- 13 Pool -- #
    main(processes=13)
    # -- 8 Pool -- #
    main(processes=8)
    # -- 4 Pool -- #
    main(processes=4)
    # -- Single -- #
    main(processes=1)
```
> Вообще говоря, скорость выполнения таких процессов сильно зависит от скорости и качества интернет соединения. Кроме того, логически более правильно здесь было бы использовать `async`, а не `multiprocessing`. Однако даже в таком примере виде прирост скорости.

На моей машине получилось вот так:
```
13 Pool: 2.6185 sec
8 Pool: 2.7000 sec
4 Pool: 3.0071 sec
Single: 7.3520 sec
```
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

### Прокси-объекты

Прокси-объекты называются **`общими объектами`**, которые находятся в другом процессе. Этот объект также называется прокси. У нескольких прокси-объектов может быть похожий референт. Прокси-объект состоит из различных методов, которые используются для вызова соответствующих методов его референта. Ниже приведен пример прокси-объектов.

Пример –
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

### Использование класса Pool
```python
from multiprocessing import Pool  
  
def f(x):  
    return x + x  
    
if __name__ == '__main__':  # обязательная проверка (без нёё ошибка)
    with Pool(4) as p:  
        print(p.map(f, [1, 2, 3]))
```
Класс пула планирует выполнение с использованием политики FIFO. Это работает как карта, уменьшает дизайн. Входные данные отображаются с разных процессоров и объединяют выходные данные всех процессоров. После выполнения кода он восстанавливает вывод в виде списка или массива. Он ожидает завершения всех заданий и возвращает результат. Выполняемые процессы помещаются в память, а другие неисполняемые процессы удаляются из памяти.
```python
import time  
import multiprocessing
	def is_prime(n):  
      if (n <= 1) :   
          return 'not a prime number'  
      if (n <= 3) :   
          return 'prime number'  
            
      if (n % 2 == 0 or n % 3 == 0) :   
          return 'not a prime number'  
      
      i = 5  
      while(i * i <= n) :   
          if (n % i == 0 or n % (i + 2) == 0) :   
              return 'not a prime number'  
          i = i + 6  
      
      return 'prime number'def multiprocessing_func(x):  
    time.sleep(2)  
    print('{} is {} number'.format(x, is_prime(x)))  
      
if __name__ == '__main__':  
    starttime = time.time()  
    pool = multiprocessing.Pool()  
    pool.map(multiprocessing_func, range(1,10))  
    pool.close()  
    print()  
    print('Time taken = {} seconds'.format(time.time() - starttime))
```
Время, затрачиваемое на вычисления, было сокращено с 18,02 до` 6,07` с использованием класса Pool.

_____________
#### Links
[[Python]]
https://pythonpip.ru/osnovy/mnogoprotsessornost-python