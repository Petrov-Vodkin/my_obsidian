2021-03-20 17:24
#массивы
# Массивы Bash
[](https://habr.com/ru/company/ruvds/blog/413725/)
```bash
arr=()			# Создание пустого массива
arr=(1 2 3)		# Инициализация массива
${arr[2]}		# Получение третьего элемента массива
${arr[@]}		# Получение всех элементов массива
${!arr[@]}		# Получение индексов массива
${#arr[@]}		# Вычисление размера массива
arr[0]=3		# Перезапись первого элемента массива
arr+=(4)		# Присоединение к массиву значения
str=$(ls)		# Сохранение вывода команды ls в виде строки
arr=( $(ls) )	# Сохранение вывода команды ls в виде массива имён файлов
${arr[@]:s:n}	# Получение элементов массива начиная с элемента с индексом s до элемента с индексом s+(n-1)
```
***RANGE***[](https://linuxhint.com/bash_range/)
```bash
"генератор массива"
list=$(seq 1 10)
```
```bash
END=number
for i in $(seq 1 $END); do echo $i; done
```
```bash
echo {1..5}
```
_____________
#### Links
[[Скрипт bash]]