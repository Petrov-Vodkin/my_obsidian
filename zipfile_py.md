2021-11-17 14:25
[docs ru](https://docs-python.ru/standart-library/modul-zipfile-python/obekt-zipfile-modulja-zipfile/)
# zipfile
`mode`:
-  r - файл будет открыт для чтения;
-  w - если файл существует, то он будет уничтожен и вместо него будет создан новый файл;
-  a - существующий файл будет открыт в режиме добавления в конец.

`compression` определяет метод сжатия, который должен использоваться при записи в архив. Он принимает одно из значений: ZIP_STORED или ZIP_DEFLATED. По умолчанию используется значение ZIP_STORED. Последний аргумент
`allowZip64` позволяет разрешить использование расширений ZIP64, которые дают возможность создавать архивы размером больше 2 гигабайт. По умолчанию равен False.
Итак, давайте откроем наш ранее созданный архив для чтения:
```py
# 					**Является ли файл zip архивом:**
zipfile.is_zipfile('xml_healer.zip') # True
# 							**Чтение архива:**
z = zipfile.ZipFile('xml_healer.zip', 'r')
# 						**Просмотр содержимого:**
z.printdir()
# 						**Извлечение содержимого:**
z.extract('file') # Извлечь файл из архива
z.extractall()    # Извлечь все файлы
# 							**Запись содержимого:**
with zipfile.ZipFile('spam.zip', 'w') as myzip:
    myzip.write('file')

z = zipfile.ZipFile('spam.zip', 'w')
z.write('file')
z.close()
```
Для записи всех файлов в директории можно воспользоваться функцией os.walk:
```py
import zipfile
import os

z = zipfile.ZipFile('spam.zip', 'w')        # Создание нового архива
for root, dirs, files in os.walk('folder'): # Список всех файлов и папок в директории folder
for file in files:
   z.write(os.path.join(root,file))         # Создание относительных путей и запись файлов в архив

z.close()

# 						**Закрытие архива:**

z.close()
```
_____________
#### Links
-   [13.5. zipfile — Work with ZIP archives](https://docs.python.org/3.3/library/zipfile.html?highlight=zipfile#module-zipfile)
-   [Работаем с zip архивами в Python](http://sayakhov.com/blog/post/6/)
-   [Python - Добавление файлов в архив. Создание копий директорий](http://www.cyberforum.ru/python/thread1268498.html)