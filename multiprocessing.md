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
## РАБОТАЕТ(потоки)
```py
# so EASY ))) юзал с request, НО работает как ПОТОКИ
from multiprocessing.dummy import Pool as ThreadPool 

pool = ThreadPool(10) # say
all = pool.map(check_vulnerability, packages)
```
_____________________________________________________
### Передать  аргумент через [процесс](https://pythonim.ru/moduli/multiprocessing-python)
нужно использовать аргумент ключевого слова args. Следующий код будет полезен для понимания использования класса Process.  
```py
from multiprocessing import Process  
  
  
def print_func(continent='Asia'):  
    print('The name of continent is : ', continent)  
  
if __name__ == "__main__":  # confirms that the code is under main function  
 	names = ['America', 'Europe', 'Africa']  
    procs = []  # Pool процессов
  
    # instantiating process with arguments  
 	for name in names:  
		# создаем процесс для каждого аргумента из names  
 		proc = Process(target=print_func, args=(name,)) 
        procs.append(proc)  # добавляем процесс в пулл
        proc.start()  		# стартуем выполнение процесса
  
 	for proc in procs:  
        proc.join()			# дожидаемся выполнения процесса

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