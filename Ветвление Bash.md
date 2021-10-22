2021-03-19 13:28
#bash
# Ветвление Bash

```bash
"Условный оператор IF"
									|	# !/bin/bash
if команда_условие  				|	if [[ $1 > 2 ]]
then  								|	then
команда  							|	echo $1" больше 2"
else  								|	else
команда  							|	echo $1" меньше 2 или 2"
fi 		"завершение блока команд"	|	f1
```

```bash
" сравнение строк"
-z <строка>						# строка пуста
-n <строка>						# строка не пуста
<str> == <str> && <str> != <str>
> , <, = 
" сравнене чисел"
-eq  		# равно
-ne			# не равно
-lt			# меньше
-le			# меньше или равно
-gt			# болше
-ge			# бльше или рано
"условия файлы"			
-e <путь> # путь существует
-f <путь> # это фаил
-d <путь> # это директория
-s <путь> # размер файла > 0
-x <путь> # фаил исполняемый
```
```bash
"CASE"
#!/bin/bash

if [[ $# -ne 2 ]] 
then
  echo "You should specify exactly two arguments!"
else  
  case $1 in
    1)
      echo "Creating file $2"
      touch $2
      ;;
    2)
      echo "Creating dir $2"
      mkdir $2
      ;;
    *)
      echo "Wrong value!"
  esac
fi
```
```bash
case ${my_var} in
  [0-9]|[1-4][0-9]|50) temp_color=0;;  # 0-50
  5[1-9]|6[0-9]|7[1-5]) temp_color=1;; # 51-75
  7[6-9]|8[0-9]) temp_color=2;;        # 76-89
  9[0-4]) temp_color=3;;               # 90-94
  *) temp_color=4;;                    # 95-100
esac
```
#### Циклы[](https://losst.ru/tsikly-bash)
```bash
#!/bin/bash

for i in 1 2 3 4 5
do
  file_name=file${i}.txt
  if [[ -e $file_name ]]
  then
    continue
  fi
  echo "Creating file $file_name"
  touch $file_name
done
```
**Выражение последовательности в Bash**[](https://andreyex.ru/linux/vyrazhenie-posledovatelnosti-v-bash-diapazon/)
```bash
{START..END\[..INCREMENT\]}

for i in {3..0}		# от 3 до 0
for i in {0..20..5} # о  0 до 20 с шагом 5
do
  echo "Номер: $i"
done
```
```bash
#!/bin/bash

again=yes 
while [ "$again" == "yes" ] 
do
  echo "Please enter a name:"
  read name
  echo "The name you entered is $name"

  echo "Do you wish to continue? (yes/no)"
  read again
done
```
[***бесконечный цикл***](https://losst.ru/tsikly-bash)
```bash
while :  
do  
	echo "Бесконечный цикл bash, для выхода нажмите Ctrl+C"  
done
```
_____________
#### Links
[[Bash]]