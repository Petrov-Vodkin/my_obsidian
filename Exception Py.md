2021-11-12 13:48
#tag
# Exception Py
```py
# самопальный класс
class NonPositiveError(Exception):  
    pass  
  
# предок list (аналогичен ему)=> но аппендит ТОЛЬКО числа(+ они > 0) иначе ошибка NonPositiveError
class PositiveList(list):  
    def append(self, integer) -> None:  
        if integer > 0:  
            super().append(integer)  
        else:  
            raise NonPositiveError()
```
_____________
#### Links
[[Python]]