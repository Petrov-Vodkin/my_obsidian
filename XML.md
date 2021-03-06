2021-11-18 12:03
#tag
# XML
-сами определяем теги. Исп-ся для хранения данных. 
### [xmltodict](https://pythonim.ru/osnovy/preobrazovanie-xml-v-json-i-dict-v-python)
XML в JSON  
```py
import xmltodict
import pprint
import json

# V1
my_xml = """
    <audience>
      <id what="attribute">123</id>
      <name>Shubham</name>
    </audience>
"""

pp = pprint.PrettyPrinter(indent=4)
pp.pprint(json.dumps(xmltodict.parse(my_xml)))

# V2

with open('person.xml') as fd:
    doc = xmltodict.parse(fd.read())

pp = pprint.PrettyPrinter(indent=4)
pp.pprint(json.dumps(doc))

# XML in Dict  
my_dict = xmltodict.parse(my_xml)
print(my_dict['audience']['id'])
print(my_dict['audience']['id']['@what'])

# Поддержка пространств имен в XML  - process_namespaces=True
with open('person.xml') as fd:
    doc = xmltodict.parse(fd.read(), process_namespaces=True)

```
#### преобразовать JSON в XML  
```py
import xmltodict

student = {
  "data" : {
    "name" : "Shubham",
    "marks" : {
      "math" : 92,
      "english" : 99
    },
    "id" : "s387hs3"
  }
}
print(xmltodict.unparse(student, pretty=True))
```
----------------------------------------
### ElementTree
```py
import xml.etree.ElementTree as ET‭


tree‭ = ‬ET.parse‭('‬items.xml‭') # создаём структуру дерева при помощи функции
root‭ = ‬tree.getroot‭()		# получаем его корневой элемент

#‭ ‬атрибут отдельного элемента
print‭('‬Item‭ ‬#2‭ ‬attribute:‭')
print(root‭[‬0‭][‬1‭]‬.attrib‭)

#‭ ‬атрибуты всех элементов
print‭('\‬nAll attributes:‭')
for elem in root:‭
	for subelem in elem:
		print(subelem.attrib)

#‭ ‬данные отдельного элемента
print‭('\‬nItem‭ ‬#2‭ ‬data:‭')
print(root‭[‬0‭][‬1‭]‬.text‭)

#‭ ‬данные всех элементов
print‭('\‬nAll item data:‭')
for elem in root:‭
	for subelem in elem:
		print(subelem.text)
```
#### Подсчёт элементов в документе XML
```py
import xml.etree.ElementTree as ET‭

tree‭ = ‬ET.parse‭('‬items.xml‭')
root‭ = ‬tree.getroot‭()		# получаем корневой элемент
print(len(root‭[‬0‭]))			#‭ ‬общее количество элементов
 ```
_________________________________
### minidom
Этот модуль использует функцию `parse`,‭ ‬чтобы создать объект DOM из нашего файла XML.‭
```py
# имя файла может быть строкой,‭ ‬содержащей путь к файлу или объект файлового типа
xml.dom.minidom.parse(filename_or_file‭[‬,‭ ‬parser‭[‬,‭ ‬bufsize‭]])
# возвращает документ,‭ ‬который можно обработать как тип XML
```
Итак,‭ ‬мы можем использовать функцию `getElementByTagName‭()`‬,‭ ‬чтобы найти определённый тэг.
Поскольку каждый узел можно рассматривать как объект,‭ ‬мы можем получить доступ к атрибутам и тексту элемента через свойства объекта.‭ ‬В примере ниже мы добрались до атрибутов и текста отдельного узла и всех узлов вместе.
```py
from xml.dom import minidom

mydoc‭ = ‬minidom.parse‭('‬items.xml‭')			#‭ ‬обработка файла xml по имени

items‭ = ‬mydoc.getElementsByTagName‭('‬item‭')	# получаем элемент по тегу

#‭ ‬атрибут отдельного элемента
print‭('‬Item‭ ‬#2‭ ‬attribute:‭')
print(items‭[‬1‭]‬.attributes‭['‬name‭']‬.value‭)

#‭ ‬атрибуты всех элементов
print‭('\‬nAll attributes:‭')
for elem in items:‭
	print(elem.attributes‭['‬name‭']‬.value‭)

#‭ ‬данные отдельного элемента
print‭('\‬nItem‭ ‬#2‭ ‬data:‭')
print(items‭[‬1‭]‬.firstChild.data‭)
print(items‭[‬1‭]‬.childNodes‭[‬0‭]‬.data‭)
#‭ ‬данные всех элементов
print‭('\‬nAll item data:‭')
for elem in items:‭
	print(elem.firstChild.data‭)
 ```
 Если мы хотим использовать уже __`открытый файл`__,‭ ‬можно просто передать наш файловый объект функции `parse`:
```py
datasource‭ = ‬open‭('‬items.xml‭')
#‭ ‬обработка открытого файла
 mydoc‭ = ‬parse(datasource‭)
```
Также,‭ ‬если данные XML уже были загружены как __`строка`__,‭ ‬то мы могли бы использовать вместо этого функцию `parseString‭()`‬.‭
#### Подсчёт элементов в документе XML
```py
from xml.dom import minidom

mydoc‭ = ‬minidom.parse‭('‬items.xml‭')			#‭ ‬обработка файла xml по имени
items‭ = ‬mydoc.getElementsByTagName‭('‬item‭')	# найти элемент 
print(len(items‭))							#‭ ‬общее количество элементов
```
_____________
#### Links
https://gitjournal.tech/python_xml/