2021-07-07 16:45
#tag 
# Файлы в python
### Менеджер контекста [[Контекстный менеджер]]
```py
with open('readme.txt', 'w') as file:
    file.write('hello world')
```
### Метод open() 
```py
f = open(file_name, access_mode)
```

| Режим | Описание |
|-------|----------|
| r | Только для чтения.|
| w | Только для записи. Создаст новый файл, если не найдет с указанным именем.
| rb | Только для чтения (бинарный).
| wb |Только для записи (бинарный). Создаст новый файл, если не найдет с указанным именем.
| r+ | Для чтения и записи.
| rb+ | Для чтения и записи (бинарный).
| w+ | Для чтения и записи. Создаст новый файл для записи, если не найдет с указанным именем.
| wb+ | Для чтения и записи (бинарный). Создаст новый файл для записи, если не найдет с указанным именем.
| a| Откроет для добавления нового содержимого. Создаст новый файл для записи, если не найдет с указанным именем.
| a+| Откроет для добавления нового содержимого. [Создаст новый файл](https://pythonru.com/osnovy/prostoj-sposob-sozdat-fajl-v-python) для чтения записи, если не найдет с указанным именем.
| ab| Откроет для добавления нового содержимого (бинарный). Создаст новый файл для записи, если не найдет с указанным именем.
| ab+| Откроет для добавления нового содержимого (бинарный). Создаст новый файл для чтения записи, если не найдет с указанным именем.

### Чтение 
```py
			" Функция read()"
f = open('example.txt','r')
f.read(7)  # чтение 7 символов из example.txt
'This is '

			"Функция readline()"
x = open('test.txt','r')
x.readline()  # прочитать первую строку
This is line1.
x.readline(2)  # прочитать вторую строку
This is line2.
x.readlines()  # прочитать все строки
['This is line1.','This is line2.','This is line3.']
			
```
### Запись:  write()
```py
# файла `xyz.txt` не существует. Он будет создан при попытке открыть его в режиме чтения
f = open('xyz.txt','w')  # открытие в режиме записи
f.write('Hello \n World')  # запись Hello World в файл
Hello
World
f.close()  # закрытие файла
```

### Переименование файлов в Python: rename()
[модуль os](https://pythonru.com/osnovy/rabota-s-fajlami-v-python-s-pomoshhju-modulja-os).
```py
import os
os.rename(src,dest)
#   `src` = файл, который нужно переименовать
#   `dest` = новое имя файла
								**Пример**
>>> import os
>>> # переименование xyz.txt в abc.txt
>>> os.rename("xyz.txt","abc.txt")
```
### Методы файла в Python
|				|				|
|-----------------|---------------|
|`file.close()`|закрывает открытый файл
|`file.fileno()`|возвращает целочисленный дескриптор файла
|`file.flush()`|очищает внутренний буфер
|`file.isatty()`|возвращает True, если файл привязан к терминалу
|`file.next()`|возвращает следующую строку файла
|`file.read(n)`|чтение первых n символов файла
|`file.readline()`|читает одну строчку строки или файла
|`file.readlines()`|читает и возвращает список всех строк в файле
|`file.seek(offset[,whene])`|устанавливает текущую позицию в файле
|`file.seekable()`|проверяет, поддерживает ли файл случайный доступ. Возвращает `True`, если да
|`file.tell()`|возвращает текущую позицию в файле
|`file.truncate(n)`|уменьшает размер файл. Если n указала, то файл обрезается до n байт, если нет — до текущей позиции
|`file.write(str)`|добавляет [строку](https://pythonru.com/osnovy/stroki-python) `str` в файл
|`file.writelines(sequence)`|добавляет последовательность строк в файл
_____________
#### Links
[youtube](https://www.youtube.com/watch?v=fUJT0TVfcFo&list=PLlWXhlUMyooaeSj8L8tVVbtUo0WCO4ORR&index=14)
https://pythonru.com/osnovy/fajly-v-python-vvod-vyvod

[[Python]]