2021-06-01 19:30
#итератор
# itertools
```py
						# расширить список
from itertools import *
a = [10, 20, 30]
print(list(chain(a, [40, 50, 60]))) # Выход: [10, 20, 30, 40, 50, 60]
```
```py
				# Большие коллекции: 100 миллионов четверок (`4`)
from itertools import repeat
lots_of_fours = repeat(4, times=100_000_000) #  этот итератор занимает 56 байт

```
_____________
#### Links
[[Библиотека Python]]
[Портал - Примеры кода с описанием](http://espressocode.top/extending-list-python-5-different-ways/)