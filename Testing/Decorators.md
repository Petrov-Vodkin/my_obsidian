2020-12-04 14:59
#декоратор 
# Декораторы python
#### [[Руководство по декораторам]]
#### [[functool]]
**`Декораторы`** — это, по сути, "обёртки", которые дают нам `возможность изменить поведение функции, не изменяя её код`.

[функции в python](https://pythonworld.ru/tipy-dannyx-v-python/vse-o-funkciyax-i-ix-argumentax.html) являются объектами, соответственно, их `можно возвращать` из другой функции или `передавать в качестве аргумента`. Также следует помнить, что функция в python может быть определена и внутри другой функции.
```py
def timeit(func):						# принемает декорируемую ф-ю 
	def wrapper(*args, **kwargs):		# принемает аргументы декорируемой ф-ии 
		start = datetime.now()			# текущее время
		result = func(*args, **kwargs)	# run func (для которой замеряем время выполнения)
		print(datetime.now() - start)	# подсчёт времени
		return result					# 
	return wrapper
	
@timeit									# == timeit(you_func)
def you_func(n):
	print(n)
	
					'OR'
@contextlib.contextmanager  
def report_time(test):  
    t0 = time.time()  
    yield  
	print("Time needed for `%s' called: %.2fs"  
	% (test, time.time() - t0))  
  
# при вызове ф-я может быть как саргуметнами так и без них  
with report_time("serialized_trade"):  
    requests.get(URL)
```
Смысл паттерна Декоратор заключается в том, что некоторая функция заворачивается в другую функцию, приобретая от нее новые возможности. Например, так можно вести логи, вводить пред- и постусловия, добавлять методы для классов.
### [Встроенные декораторы](https://python-scripts.com/decorators)
`@classmethod`
принимает единственный параметр: function — Функция, которую нужно преобразовать в метод класса. И возвращает метод класса для данной функции[](https://pythonstart.ru/osnovy/classmethod-staticmethod-python)
```py
from datetime import date

# random Person
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod # создаём класс внутри класса 
    def fromBirthYear(cls, name, birthYear): # принимает класс, имя и год рождения, 
        return cls(name, date.today().year - birthYear)

    def display(self):
        print(self.name + "'s age is: " + str(self.age))

person = Person('Adam', 19)
person.display() 								# Adam's age is: 19  
person1 = Person.fromBirthYear('John',  1985)
person1.display() 								# John's age is: 31 
```
`@staticmethod`
это методы, привязанные к классу, а не к его объекту. Они не требуют создания экземпляра класса. Они не зависят от состояния объекта. 

`@property` used for:
 - Конвертация метода класс в атрибуты только для чтения;
 - Как реализовать сеттеры и геттеры в атрибут
```py
class Person(object):
 """"""
	 def __init__(self, first_name, last_name):
		 """Конструктор"""
		 self.first_name = first_name
		 self.last_name = last_name

	 @property
	 def full_name(self):
		 """
		 Возвращаем полное имя
		 """
		 return "%s %s" % (self.first_name, self.last_name)
```
В данном коде мы создали два класса атрибута, или свойств: **self.first_name** и **self.last_name**.  
Далее мы создали метод **full_name**, который содержит декоратор `@property`. Это позволяет нам использовать следующий код в сессии интерпретатора:
```py
person = Person("Mike", "Driscoll")
print(person.full_name) # Mike Driscoll
print(person.first_name) # Mike

person.full_name = "Jackalope"
Traceback (most recent call last):
 File "<string>", line 1, in <fragment>
AttributeError: can't set attribute
```
### Decorate class 
```py
from typing import Type


def auto_str(c: Type):
  def str(self):
    variables = [f"{k}={v}" for k, v in vars(self).items()]
    return f"{c.__name__}({', '.join(variables)})"
  c.__str__ = str

  return c


class Sample1:
  def __init__(self, a, b):
    self.a = a
    self.b = b


@auto_str
class Sample2(Sample1):
  def __init__(self, a, b, c):
    super().__init__(a, b)
    self.c = c


print(str(Sample2(1, 2, 3)))
```
#### Links
[[Python]] | [[Fixtures]]

https://youtu.be/Ss1M32pp5Ew?list=PLlWXhlUMyooYqypXIju-5czBtppKaWimP
