2021-03-22 17:59
#функции
# Функции в Bash
### Арифметика
let “переменная = выражение”
```bash
let “c = 1 + 1”
let “c = a + b”
```
#### Операции:
`+, -, /, *` стандартные	`%` остаток от деления  `**`  возведение в степень
```bash
#!/bin/bash

print_sum () { let "sum = $1 + $2"; echo "$1 + $2 = $sum"; } 
print_mult () { let "mult = $1 * $2"; echo "$1 * $2 = $mult"; } 

print_sum 2 2
print_mult 5 5
print_mult 6 6
```
#### Cокращение:
let “a=a+b” эквивалентно let “a+=b”
#### Внешние программы
```bash
переменная=`программа` # Синтаксис

a=`echo “test”`			# Пример
files=`ls ~`
```
#### Код возврата:
0 корректное завершение
не 0 в процессе работы были ошибки
`$?`  Узнать код
`exit код`  Выйти с кодом:
```bash
touch file.txt
echo $?
```
Проверка кода возврата:
```bash
if `программа` 
then
# действия, если код 0
еlse
# действия, если код не 0
fi
```
## Функции
```bash
#!/bin/bash

files_creator ()  # dir_name file_name 
{
  full_name=$1/$2
  if [[ ! -e $1 ]]; then
     echo "Directory does not exist, creating $1"
     mkdir $1
  elif [[ ! -d $1 ]]; then
     echo "$1 is not a directory, exiting"
     exit 1
  fi
  touch $1/$2
}

again="yes"
while [[ $again == "yes" ]]; do
  read -p "Enter dir and file names: " dir_name file_name
  files_creator $dir_name $file_name
  if [[ -f $full_name ]]; then echo "Created $full_name"; fi
  read -p "Again? (yes/no): " again
done


```
```bash
# Функции с параметрами
имя_функции ()
{
# действия с $1, $2, … , $#
}
# Используем функцию
...
имя_функцииаргумент1 аргумент2 ...
...
```
```bash
# Переменные
имя_функции ()
{
var_global=1
localvar_local=1 
} 
# Используем
имя_функции
echo $var_global # выведет 1
echo $var_local # ничего не выведет
```
#### Компактная запись
```bash
func_name () { действ1;действ2;} # Компактная запись

# Актуально и в других конструкциях:
if [[ $var==”test ”]]; then
...
for i in 1 2 3 4 5; do
...
```
_____________
#### Links
[[Bash]]