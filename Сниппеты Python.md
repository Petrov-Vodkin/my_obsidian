2021-02-04 13:47
#snippets
# Сниппеты 
#### [[Трюки в Python]]
[[#Date time]]
#### Тернарный оператор
```py
# ex1
x, y = 25, 50
big = x if x < y else y

# ex2
for item in x:
    print('OK =>', item) if item.endswith(suffix) else print('NO =>', item)

```
#### File
```py
# Проверка существования файла: Метод перебора
import os.path
from os import path

def check_for_file():
 print("File exists: ",path.exists("data.txt"))

if __name__=="__main__":
   check_for_file()
```
```py
				# запись слов вфаил с новой строки "\n"
f = open("test1.txt", "w")  
lines = ["Line 1", "Line 2", "Line 3"]  
contents = "\n".join(lines)  
f.write(contents)  
f.close()
```
```py
					# Рекурсивный обход дтректорий
for current_dir, dirs, files in os.walk("."):  
    print(current_dir, dirs, files)

```
#### Объекты / Переменные
```py
#  использование памяти объекта:
import sys
a = 20
print(sys.getsizeof(a)) #28
```
#### Input Output
```py
		# Считать все строки по одной из стандартного потока ввода
import sys

for line in sys.stdin:
    line = line.rstrip()
    # process line
```
```py
			## Прием двух целых чисел в качестве входных данных
a,b = map(int,input().split())
print("a:",a)
print("b:",b)

			## Прием списка в качестве входных данных
arr = list(map(int,input().split()))
print("Input List:",arr)
```
#### Web/Net
```python
			# "Получает url страницы и печатает их (без общего префикса):"
page = get_page("https://tutsplus.com/authors/gigi-sayfan")
articles=get_page_articles(page)
prefix= "https://code.tutsplus.com/tutorials"
for a in articles:
	print (a [len (prefix):])
```

```py
# безконечность
infinity = float("inf")
```
### Date & time
#### Вычисление `времени` выполнения в оболочке 
[[Decorators]]
```py
@contextlib.contextmanager  
def report_time(test):  
    t0 = time.time()  
    yield  
 print("Time needed for `%s' called: %.2fs"  
 % (test, time.time() - t0))  
  
# при вызове ф-я может быть как саргуметнами так и без них  
with report_time("serialized_trade"):  
    requests.get(URL)
# МЕТОД 1
import datetime
start = datetime.datetime.now()
"""
CODE
"""
print(datetime.datetime.now()-start)

# МЕТОД 2
import time
start_time = time.time()
main()
print(f"Total Time To Execute The Code is {(time.time() - start_time)}" )

# МЕТОД 3
import timeit
code = '''
## Фрагмент кода, чье время выполнения подлежит измерению
[2,6,3,6,7,1,5,72,1].sort()
'''
print(timeit.timeit(stmy = code,number = 1000))
```
#### Дата `-` `+` дни
```py
# принимает дату в строковом формате - возвращать дату на неделю позже
from datetime import datetime, timedelta
# datetime.date для хранения даты; datetime.timedelta прибавления дней к дате
def week_after(d):
    date = datetime.strptime(d,'%d/%m/%Y') + timedelta(days=7)
    return date.strftime('%d/%m/%Y')
# d.year, d.month, d.day 				# выводит как числа, без ведущих нулей
# ny_2011 = datetime.date(2011, 2, 1)	# создали дату: 1 февраля 2011 года
# delta = datetime.timedelta(days=2) 	# дельта в 2 дня  
# now_date = now_date + delta 			# + 2 дня в формате (yyyy-mm-dd)  
# now_date = now_date - delta 			# - 2 дня назад в формате (yyyy-mm-dd)
```
###  Калькулятор без if-else / 
```py
import operator

action = {
  "+" : operator.add,
  "-" : operator.sub,
  "/" : operator.truediv,
  "*" : operator.mul,
  "**" : pow
}

print(action['*'](5, 5))    # 25
```
### `try`, `except` и `finally`:
```py
# Пример 2
try:    
    #если файл не существует, будет выброшено исключение.     
    fileptr = open("file.txt","r")    
except IOError:    
    print("File not found")    
else:    
    print("The file opened successfully")    
    fileptr.close()

# Пример 3
try:
  fptr = open("data.txt",'r')
  try:
    fptr.write("Hello World!")
  finally:
    fptr.close()
    print("File Closed")
except:
  print("Error")
```
## Списки
#### Сортировка списка строкске:
```py
list1 = ["Karl","Larry","Ana","Zack"]
list1.sort()				# Метод 1: sort()
sorted_list = sorted(list1) # Метод 2: sorted()
```
### Генератор списков с If и Else
```py
## List Comprehension with if and else
["Divided by 5" if i%5==0 else i for i in range(1,20)]
### FizzBuzz Implementation Threw the same
['FizzBuzz' if i%3==0 and i%=5=0 else 'Fizz' if i%3==0 else 'Buzz' if i%5==0 else i for i in range(1,20)]
```
####  Сложение элементов двух списков
```py
maths = [59, 64, 75, 86]
physics = [78, 98, 56, 56]
# Метод перебора
list1 = [maths[0]+physics[0], maths[1]+physics[1], maths[2]+physics[2], maths[3]+physics[3]]
# Генератор списков
list1 = [x + y for x,y in zip(maths,physics)]
# Использование методов map
import operator 
all_devices = list(map(operator.add, maths, physics))
# Использование библиотеки Numpy
import numpy as np
list1 = np.add(maths,physics)
```
#### Самые часто встречающиеся в списке
```py
def most_frequent(nums):
  return max(set(nums), key = nums.count)
```
#### Поиск дублей в списках
```py
def has_duplicates(lst):
    return len(lst) ≠ len(set(lst))

x = [1,2,2,4,3,5]
y = [1,2,3,4,5]
has_duplicates(x) # True
has_duplicates(y) # False
```
### Рандомизация содержимого списка shuffle:
```py
from random import shuffle
list_example = [1,2,3,4,5,6,7,8]
shuffle(list_example)
```
### Словари
#### Сортировка списка словарей
```py
dict1 = [{"Name":"Karl", "Age":25}, {"Name":"Lary", "Age":39}, {"Name":"Nina", "Age":35}]
## Использование sort()
dict1.sort(key=lambda item: item.get("Age"))
# Сортировка списка с помощью itemgetter
from operator import itemgetter
f = itemgetter('Name')
dict1.sort(key=f)
# Функция sorted для итерируемых элементов
dict1 = sorted(dict1, key=lambda item: item.get("Age"))
```
В Python 3 версии оператор `/=` выполнит вещественное деление (т.е. вернется результат с точкой), а оператор `//` целочисленное.

### Строки
**Удалить пробелы из строки (string) “aaa bbb ccc ddd eee”?**
```python
#Функция `join()`:
s='aaa bbb ccc ddd eee'
s1=''.join(s.split())
#Генератор списка (list comprehension):
s1=str(''.join(([i for i in s if i!=' '])))
#Функция `replace()`:
s1 = s.replace(' ','')
```
```py
# получить случайную строку из txt
import random
with open ('myfile.txt', 'r') as file:
    lines = file.readlines()
    print(random.choice(lines))
```
### Генераторы
```py
# генератор уникальных имен файла на основе времени
from time import time  


def gen_filename():  
    while 1:  
        t = int(time()) * 1000  
		yield f'{t}.jpeg'
		# можно ещё добавить код или ещё yield
```
### Числа
[link](https://pythonist.ru/proverka-chisla-na-prostotu/)
```py
			# V1 Проверка: ПРОСТОЕ ЧИСЛО
a = int(input("Введите число: "))
k = 0
for i in range(2, a // 2+1):
    if (a % i == 0):
        k = k+1
if (k <= 0):
    print("Число простое")
else:
    print("Число не является простым")

			# V2 Проверка: ПРОСТОЕ ЧИСЛО по теореме Вильсона
def primes():
    i, f = 2, 1  # число и факториал предыдущего числа
    while True:
        if (f + 1) % i == 0:  # проверяем на простоту по теореме Вильсона через факториал
            yield i
        f, i = f * i, i + 1  # сначала пересчитываем факториал для текущего числа, затем увеличиваем число
```
_____________
#### Links
[[Python]]