2021-05-26 13:45
#tag
# datetime
```python
import datetime

dt_obj =datetime.datetime.now()
dt_string = dt_obj.strftime("%d-%b-%Y (%H:%M:%S.%f)")
print('Текущее время : ', dt_string)
# Текущее время :  14-Nov-2020 (17:18:09.890960)
```
```python
import datetime

dt_obj =datetime.datetime.now()
dt_string = dt_obj.strftime("%H:%M:%S.%f - %b %d %Y")
print('Текущее время : ', dt_string)
# Текущее время :  17:20:28.841390 - Nov 14 2020
```
```py
%a	# День недели, короткий вариант				Wed
%A	# Будний день, полный вариант				Wednesday
%w	# День недели числом 0-6, 0 — воскресенье	3
%d	# День месяца 01-31							31
%b	# Название месяца, короткий вариант			Dec
%B	# Название месяца, полное название			December
%m	# Месяц числом 01-12						12
%y	# Год, короткий вариант, без века			18
%Y	# Год, полный вариант						2018
%H	# Час 00-23									17
%I	# Час 00-12									05
%p	# AM/PM										PM
%M	# Минута 00-59								41
%S	# Секунда 00-59								08
%f	# Микросекунда 000000-999999				548513
%z	# Разница UTC								+0100
%Z	# Часовой пояс								CST
%j	# День в году 001-366						365
%U	# Неделя числом в году, Воскресенье первый день недели, 00-53	52
%W	# Неделя числом в году, Понедельник первый день недели, 00-53	52
%c	# Локальная версия даты и времени			Mon Dec 31 17:41:00 2018
%x	# Локальная версия даты						12/31/18
%X	# Локальная версия времени					17:41:00
%%	# Символ “%”								%
```
```py
import datetime  
a = datetime.datetime.today()  
print(a.year)  
print(a.month)  
print(a.day)  
print(a.hour)  
print(a.minute)  
print(a.second)  
print(a.microsecond)
```
_____________
#### Links
[[Библиотека Python]]
https://pythonru.com/primery/kak-ispolzovat-modul-datetime-v-python
https://pythonim.ru/chisla/klass-date-v-python
