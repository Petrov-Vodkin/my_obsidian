2021-03-16 17:12
#csv #saveincsv
# CSV
CSV (значения, разделенные запятыми) — это наиболее распространенный формат файлов, который широко поддерживается многими платформами и приложениями. Используйте модуль csv из стандартной библиотеки Python.
### [Как сохранить словарь Python в файл CSV?](https://pythononline.ru/question/kak-sohranit-slovar-python-v-fayl-csv)
```py
import csv
my\_dict = {'1': 'aaa', '2': 'bbb', '3': 'ccc'}  
with open('test.csv', 'w') as f: 
for key in my\_dict.keys(): 
	f.write("%s,%s\\n"%(key,my\_dict\[key\]))  
```
Модуль csv содержит метод DictWriter, для которого требуется записать имя файла csv и объект списка, содержащий имена полей. 
Метод `writeheader()` **записывает первую строку в CSV-файл как имена полей**. Последующий цикл for записывает каждую строку в форме csv в файл csv.  
```py
import csv
csv_columns = ['No','Name','Country']
dict_data = [
{'No': 1, 'Name': 'Alex', 'Country': 'India'},
{'No': 2, 'Name': 'Ben', 'Country': 'USA'},
{'No': 3, 'Name': 'Shri Ram', 'Country': 'India'},
{'No': 4, 'Name': 'Smith', 'Country': 'USA'},
{'No': 5, 'Name': 'Yuva Raj', 'Country': 'India'},
]
csv_file = "Names.csv"
try:
    with open(csv_file, 'w') as csvfile:
        writer = csv.DictWriter(csvfile, fieldnames=csv_columns)
        writer.writeheader()
        for data in dict_data:
            writer.writerow(data)
except IOError:
    print("I/O error")
```
_____________
#### Links
[[Python]] [[Dict]]