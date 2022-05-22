2021-04-27 15:55
#tag
# Итератор Гениратор
### Итератор
`Итератор` - объект, принимающий коллекцию эдементов`iter(elems)`, имееет метод `elems.__next__` || `next(elems)`, возвращает элемент, можно обойти 1н раз 
`Итерация` - процесс перебора 
```py
list = [somf] 
for i in lst:  
    print(i)  
  					# цикл "под капотом"
it = iter(lst)  			# создаём объект итераор
while True:  			
    try:  				
        i = next(it)  		# 
        print(i)  
    except StopIteration:	# 
        break
```
`Итерируемый объект` - об-к, потдерживает возможность "обхода" поочерёдно своих элементов, возвращает свои элементы по одному.  Создать объект `итератор` => `iter(Итерируемый объект)`
```python						
						# реализация простого объекта итератора
from random import random  
  
  
class RandomIterator:  
    def __iter__(self):  	# возврвщает следующий элемент 
        return self  
  
 	def __init__(self, k):  
        self.k = k  		# кол-во случаймых элем-в
        self.i = 0  		# инициализируем счётчик
  
 	def __next__(self):  		# чтобы экзкмпляр cl стал итераторам НЕОБХОДИМ метод next
        if self.i < self.k: #  
            self.i += 1  	# 
 			return random()  
        else:  
            raise StopIteration  
  
for x in RandomIterator(10):  
    print(x)
```
```python
			""" итератор, который возвращает серию чисел """
class Series(object):
    def __init__(self, low, high):
        self.current = low
        self.high = high

    def __iter__(self):
        return self

    def __next__(self):
        if self.current > self.high:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1

n_list = Series(1,10)    
print(list(n_list))
```
### Генератор
[Генератор](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-generator-generator/ "Тип данных generator (генератор).") - это функция, которая возвращает объект [итератора](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-iterator-iterator/ "Тип данных Iterator (итератор).")
-`создает последовательность` значений `для итерации`
-`удобный синтаксис для написания итераторов`(по средствам yield) (не нужно реалиовывать класс с методами `__iter__` и `__next__`)
-генераторы могут быть двух разных типов: функции-генераторы и генераторные выражения
 
концепция отложенного исполнения, продолжим исполнение ф-ии при следующем обращении к ней

```python
# выражение-генератор: выбрасывает ИСКЛЮЧЕНИЕ StopIteration, если закончились элементы
gen = (i+1 for i in range(10)) # 'выражения-генераторы - генерируют итератор'
# элементы не хранятся в памяти(выдаёт по очереди по 1-му при вызове) next()

# на основе ф-и
def get_range(num):  
    for i in range(num):  
        yield i
# 
def squares(n):
    i=1
    while(i<=n):
        yield i**2
        i+=1
for i in squares(7):
    print(i)		
```
```python
def fib(a=0, b=1):  
    """Generator that yields Fibonacci numbers. `a` and `b` are the seed values"""  
    while True:  
        yield a  
        a, b = b, a + b  
  
  
f = fib()  
print(', '.join(str(next(f)) for _ in range(10)))
```

#### Links
[[Python]]
[you_tube](https://www.youtube.com/watch?v=vn6bV6BYm7w&list=PLwWxlJniY3NzqGM0Kjmd_zmUqqH3pUSYL&index=31)
https://pythonist.ru/iteratory-i-generatory-v-python/