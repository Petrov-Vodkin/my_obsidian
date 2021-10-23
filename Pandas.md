2021-10-12 11:05
[example_habr](https://habr.com/ru/post/486756/)
# Pandas
**Сценарий №1**  
  
Есть три файла в формате csv с данными. Для каждой строки с данными есть поле с уникальным идентификатором id. Данные из этих файлов отбираются с учетом определенных условий и заносятся в таблицу в БД, затем эти данные выводятся в отчете в виде таблицы на UI. Есть возможность выгрузки данных на UI в Excel-файл.  
  
Предположим, что условия выбора данных для отчета из файлов-исходников следующие:  
  

-   В файлах могут быть дубли по id, в отчете запись с одним и тем же идентификатором должна учитываться только один раз (при этом выбираем просто одну любую из строк с таким идентификатором из данных).
-   Строки с отсутствием данных в ячейке столбца reg_date не должны учитываться.
-   На самом деле условий отбора может быть больше, также данные могут сравниваться с уже имеющимися в системе данными и в отчет уже будут выводиться только пересекающиеся данные по id, но для примера будем учитывать только два условия, указанных выше.

  
Задача тестировщика: Проверить, что строки с нужными объектами корректно отобраны из файлов-исходников и все эти объекты выводятся в отчете на UI.  
  
Составляем сценарий для скрипта:  
  

-   Загружаем в дата-фреймы нужные данные из этих трех csv-файлов, объединяем данные в один дата-фрейм (по аналогии с union в SQL), удаляем строки с дублями по id, удаляем строки без данных в столбце reg_date.
-   Выгружаем на UI данные итогового отчета в Excel-файл, удаляем ненужные данные, загружаем данные из него во второй дата-фрейм.
-   Объединяем (merge) эти два дата-фрейма в один (по аналогии с outer join в SQL) по уникальному идентификатору и уже записываем полученные данные в Excel-файл для дальнейшего анализа.
-   Дальше уже открываем полученный файл и с помощью фильтров анализируем строки с данными и смотрим, что все строки, выбранные из файлов-исходников с учетом условий, есть в файле выгрузки данных отчета, полученном на UI.

  
В итоговом файле данные будут содержать только один столбец с идентификатором id, если названия у столбцов в разных дата-фреймах совпадали, и может быть не понятно, какие столбцы/строки из какого были файла. Поэтому я либо называю столбцы с уникальным идентификатором разными названиями в файлах, либо в каждый файл добавляю отдельный столбец «Строки из файла такого-то» и в нем проставляю значения «Да» — потом при анализе итогового Excel-файла удобно делать фильтрацию по этому столбцу, т.к. они всегда содержат значение и, фильтруя по ним, можно уже понять, какие данные расходятся в соответствующих столбцах.  
  
Пример данных из файла _example1_csv_1.csv_:  
  
![](https://habrastorage.org/r/w1560/webt/of/5j/_u/of5j_us3u07uset3ljspbbwtrr4.png)  
  
Пример данных из файла _report_UI.xlsx_:  
  
![](https://habrastorage.org/r/w1560/webt/uh/xy/1s/uhxy1sygvioxbbhlksppdpsar_e.png)  
  
Скрипт на python выглядит следующим образом:  
  

```py
# Подключаем библиотеку pandas
import pandas as pd

# Выбираем нужные столбцы из csv-файлов и загружаем в дата-фреймы
# (в файле не должно быть столбцов с одинаковым названием)
df_from_file1 = pd.read_csv('./example1_csv_1.csv', sep=';', encoding='utf-8',
                            usecols=['id', 'name', 'email', 'reg_date'])
df_from_file2 = pd.read_csv('./example1_csv_2.csv', sep=';', encoding='utf-8',
                            usecols=['id', 'name', 'email','reg_date'])
df_from_file3 = pd.read_csv('./example1_csv_3.csv', sep=';', encoding='utf-8',
                            usecols=['id', 'name', 'email', 'reg_date'])

# Объединяем предыдущие три дата-фрейма в один новый дата-фрейм 
# (по аналогии с union в SQL)
df_from_csv = pd.concat([df_from_file1, df_from_file2, df_from_file3]).\
    reset_index(drop=True)
print(df_from_csv)

# Удаляем строки с дубликатами по указанному столбцу
df_from_csv.drop_duplicates(subset='id', keep='first', inplace=True)
print(df_from_csv)

# Удаляем строки со значением NaN (нет значения) для столбца reg_date
df_from_csv = df_from_csv.dropna()
print(df_from_csv)

# Загружаем данные из Excel-файла выгрузки с UI в дата-фрейм,
# при этом указываем название листа в файле
# (предварительно копируем файл с данными в папку со скриптом)
file_excel = 'report_UI.xlsx'
df_from_excel = pd.ExcelFile(file_excel).parse('Лист1')
print(df_from_excel)

# Объединяем дата-фрейм с отобранными данными из файлов-исходников и
# дата-фрейм с данными из файла выгрузки с UI
# (по аналогии с outer join в SQL)
df = df_from_csv.merge(df_from_excel, left_on='id', right_on="Номер", how='outer')
print(df)

# Выгружаем данные в новый Excel-файл
writer = pd.ExcelWriter('Итог.xlsx')
df.to_excel(writer, 'Лист1')
writer.save()
```

  
Ограничения:  
  

-   При работе с файлами с очень большим количеством строк придется разбивать их на отдельные файлы (тут нужно пробовать, у меня редко бывают файлы более 30 000 строк).
-   Часто в файлах (обычно формата Excel) от заказчиков/аналитиков могут быть данные с пробелами или другими невидимыми знаками в ячейках, в этом случае такие скрипты использовать не получится без чистки исходных данных.
_____________
#### Links
 [Введение в библиотеку pandas: установка и первые шаги / pd 1](https://pythonru.com/biblioteki/vvedenie-v-biblioteku-pandas-ustanovka-i-pervye-shagi "Введение в библиотеку pandas: установка и первые шаги / pd 1")
 [Структуры данных в pandas / pd 2](https://pythonru.com/biblioteki/struktury-dannyh-v-pandas "Структуры данных в pandas / pd 2")
 [Возможности объектов Index в pandas / pd 3](https://pythonru.com/biblioteki/vozmozhnosti-obektov-index-v-pandas-pd-3 "Возможности объектов Index в pandas / pd 3")
 [Основные функции Pandas / pd 4](https://pythonru.com/biblioteki/osnovnye-funkcii-pandas-pd-4 "Основные функции Pandas / pd 4")
 [Not a Number — все о NaN / pd 5](https://pythonru.com/biblioteki/not-a-number-vse-o-nan-pd-5 "Not a Number — все о NaN / pd 5")
 [Иерархическое индексирование и уровни признаков / pd 6](https://pythonru.com/biblioteki/ierarhicheskoe-indeksirovanie-i-urovni-priznakov-pd-6 "Иерархическое индексирование и уровни признаков / pd 6")
 [Чтение и запись данных (cvs, txt, HTML, XML) / pd 7](https://pythonru.com/biblioteki/chtenie-i-zapis-dannyh-cvs-txt-html-xml-pd-7 "Чтение и запись данных (cvs, txt, HTML, XML) / pd 7")
 [Чтение и запись данных (Excel, Json, SQL, MongoDB) / pd 8](https://pythonru.com/biblioteki/chtenie-i-zapis-dannyh-excel-json-sql-mongodb-pd-8 "Чтение и запись данных (Excel, Json, SQL, MongoDB) / pd 8")
 [Pickle — сериализация объектов Python / pd 9](https://pythonru.com/biblioteki/pickle-serializacija-obektov-python-pd-9 "Pickle — сериализация объектов Python / pd 9")
 [Подготовка данных в pandas / pd 10](https://pythonru.com/biblioteki/podgotovka-dannyh-v-pandas-pd-10 "Подготовка данных в pandas / pd 10")
 [Трансформация данных в pandas ч.1 / pd 11](https://pythonru.com/biblioteki/transformacija-dannyh-v-pandas-ch-1-pd-11 "Трансформация данных в pandas ч.1 / pd 11")
 [Трансформация данных в pandas ч.2 / pd 12](https://pythonru.com/biblioteki/transformacija-dannyh-v-pandas-ch-2-pd-12 "Трансформация данных в pandas ч.2 / pd 12")