2021-11-17 12:53
#tag
# os
```py
import os

os.getcwd() 				# текущая директория
print(os.listdir()) 		# список файлов в текущей дирректории

os.path.exists('sd.txt')	# слдержит ли дир. фаил sd.txt
os.path.isdir('sd.txt') 	# sd.txt это dir?
os.path.isfile('sd.txt')	# sd.txt это фаил?
path.abspath('sd.txt')		# путь(адрес) файла (C:\users\sd.txt)

os.chdir('newdir')			# изменить текущую дир на newdir

os.walk('.')				# рекурсивный обход дир-й начиная с текущей "."
			# вернёт кортеж [0] имя текущей дир. 
#							[1] список из всех поддиректорий
#							[2] список из всех файлов в тек дир

for current_dir, dirs, files in os.walk("."):  
    print(current_dir, dirs, files)


```
_____________
#### Links
