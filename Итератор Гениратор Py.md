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
```py						
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
### Генератор
-`создает последовательность` значений `для итерации`
-`удобный синтаксис для написания итераторов`(по средствам yield) (не нужно реалиовывать класс с методами `__iter__` и `__next__`)
концепция отложенного исполнения, продолжим исполнение ф-ии при следующем обращении к ней

```py
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

#### Links
[[Python]]
[you_tube](https://www.youtube.com/watch?v=vn6bV6BYm7w&list=PLwWxlJniY3NzqGM0Kjmd_zmUqqH3pUSYL&index=31)