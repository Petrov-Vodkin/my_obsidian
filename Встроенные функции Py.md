2021-11-09 08:07
[doc_ru](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/)
# Встроенные функции Py
[[Map Py]] | [[sorted Py]] | [[filter Py]]
```py
reversed()	# возвращает элементы итерируемого объекта в обратном порядке.
```
### ООП
#### Z.mro()
возвращает нам список классов ровно в том порядке, в котором Python будет искать методы в иерархии классов пока не найдет нужный или не выдаст ошибку. Example: `C -> A -> B -> objec`

#### super()
```py
super(type, object-or-type)
# type - необязательно, тип, от которого начинается поиск объекта-посредника
# object-or-type - необязательно, тип или объект, определяет порядок разрешения метода для поиска
```
В версиях Python 3.x мы можем использовать super без передачи двух вышеуказанных параметров. Посмотрите на приведенный ниже фрагмент кода. 
```py
class C(B):
	def method(self, arg): 
		super().method(arg)  
```

_____________
#### Links
https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/