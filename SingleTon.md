2021-10-02 18:40
#tag
# SingleTon
Шаблон Singleton предоставляет механизм создания одного и только один экземпляра объекта, и предоставление к нему _глобальную точку доступа_.
#### Method : A [metaclass](https://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)
```python
		# init НЕ ПЕРЕЗАПИСЫВАЕТ аргументы
class Singleton(type):  
    _instances = {}  
    def __call__(cls, *args, **kwargs):  
        if cls not in cls._instances:  
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)  
        return cls._instances[cls]  
# Метаклассы – это такие классы, экземпляры которых сами являются классами  
  
class MyClass(metaclass=Singleton):  
    def __init__(self, v):  
        self.v = v
```
```python
# Если экземпляр существует, мы всегда будем использовать уже существующий объект
class Singleton(object):
    def __new__(cls):		#переопределяем метод __new__(метод для создания бъектов) 
		# hasattr(специальный метод Python, позволяющий определить, имеет ли объект определенное свойство)	
        if not hasattr(cls, 'instance'):# проверка наличия у объекта cls свойства instance
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance
s = Singleton()
print("Object created", s)
s1 = Singleton()
print("Object created", s1)
```
#### Method: A decorator
```python
def singleton(class_):
    instances = {}
    def getinstance(*args, **kwargs):
        if class_ not in instances:
            instances[class_] = class_(*args, **kwargs)
        return instances[class_]
    return getinstance

@singleton
class MyClass(BaseClass):
    pass
```
#### Method 2: A base class
```python
class Singleton(object):
    _instance = None
    def __new__(class_, *args, **kwargs):
        if not isinstance(class_._instance, class_):
            class_._instance = object.__new__(class_, *args, **kwargs)
        return class_._instance

class MyClass(Singleton, BaseClass):
    pass
```

### Отложенный экземпляр в **Singleton**
Одним из вариантов использования шаблона Singleton является отложенная инициализация. Например, в случае импорта модулей мы можем автоматически создать объект, даже если он не нужен. Отложенное создание экземпляра гарантирует, что объект создается, только тогда, когда он действительно необходим.  
В следующем примере кода, когда мы используем **s = Singleton()**, вызывается метод **__init__**, но при этом новый объект не будет создан. Фактическое создание объекта произойдет, когда мы используем **Singleton.getInstance()**.
```py
class Singleton:
    __instance = None
    def __init__(self):
        if not Singleton.__instance:
            print(" __init__ method called..")
        else:
            print("Instance already created:", self.getInstance())
    @classmethod
    def getInstance(cls):
        if not cls.__instance:
            cls.__instance = Singleton()
        return cls.__instance

s = Singleton() ## class initialized, but object not created
print("Object created", Singleton.getInstance()) # Object gets created here
s1 = Singleton() ## instance already created
```
### Singleton на уровне модуля
Все модули по умолчанию являются синглетонами из-за особенностей работы импорта в Python. Python работает следующим образом:
1.  Проверяет, был ли уже импортирован модуль.
2.  При импорте возвращает объект модуля. Если объекта не существует, то есть модуль не импортирован, он импортируется и создается его экземпляр.
3.  Когда модуль импортируется, он инициализируется. Но когда тот же модуль импортируется снова, он уже не инициализируется, что похоже на поведение Singleton, имеющим только один объект и возвращающим один и тот же объект.

Например: a module file `singleton.py`

#### Цель шаблона Singleton заключаются в следующем:  
• Обеспечение создания одного и только одного объекта класса  
• Предоставление точки доступа для объекта, который является глобальным для программы  
• Контроль одновременного доступа к ресурсам, которые являются общими
_____________
#### Links
https://webdevblog.ru/realizaciya-shablona-singleton-v-python/
[stackoverflow 5методов создания синлтонов](https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python)