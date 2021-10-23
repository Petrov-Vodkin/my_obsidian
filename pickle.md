2021-08-21 14:09
#tag
# pickle
Модуль pickle принимает любой объект Python, преобразует его в строковое представление и сохраняет в файл с помощью функции dump, такой процесс называется pickling. Процесс извлечения исходных объектов Python из сохраненного строкового представления называется unpickling.

```py
import pickle

a = 1

#Процесс pickling
pickle.dump(a, open('file.sav', 'wb'))

#Процесс unpickling
file = pickle.load(open('file.sav', 'rb'))
```
_____________
#### Links
https://nuancesprog.ru/p/11460/