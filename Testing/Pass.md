2021-01-26 14:10
#pass
# Pass
Синтаксис `Python` требует, чтобы у некоторых операторов обязательно было тело: класс, функция, условие и т. д. Но иногда необходимо, чтобы там ничего не выполнялось. В таком случае подставляют `pass`.
**_Практические кейсы использования:_**
-   Создание пользовательского класса на основе другого
    Актуально там, где имя класса несёт смысл (исключения, модели БД и т. п.):
    ```Python
    class MyException(Exception): 
        pass
    ```
-   Декорирование:
    ```Python
    @decorator
    class MyClass(OtherClass):
        pass
    ```
    ```Python
    @decorator
    def f():
        pass
    ```
-   Создание методов абстрактного класса:
    ```Python
    class MyABC(ABC):
        @abstractmethod
        def method(self):
            pass
    ```
-   Обработка исключений:
    ```Python
    try:
        f()
    except:
        pass
    ```
 - [Инструкция pass в блоке кода](https://pythononline.ru/osnovy/operator-python-pass):
```Python
def remove_evens(list_numbers):
    list_odds = []
    for i in list_numbers:
        if i % 2 == 0:
            pass
        else:
            list_odds.append(i)
    return list_odds

l_numbers = [1, 2, 3, 4, 5, 6]
l_odds = remove_evens(l_numbers)
print(l_odds)
# out: 1, 3, 5
```


#### Links
[[Selenium сниппеты]] [[Python]]