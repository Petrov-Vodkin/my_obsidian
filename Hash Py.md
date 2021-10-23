2021-09-21 18:07
[hashlib, алгоритмы хеширования в Python](https://docs-python.ru/standart-library/modul-hashlib-python/)
# Хэш Py
**`Хэш-таблица`** — это структура данных, в которой все элементы хранятся в виде пары ключ-значение, где:
-   **`ключ`** — уникальное число, которое используется для индексации значений;
-   **`значение`** — это данные, которые с этим ключом связаны.  являются целыми числами, которые используются для быстрого сравнения ключей во время поиска по словарю.

`Хэш Функция ` - возвращает хеш-значение объекта (если оно есть):**`hash(object)`**.
__eq__ и __hash__

Модуль [hashlib](https://docs-python.ru/standart-library/modul-hashlib-python/), включенный в стандартную библиотеку Python, представляет собой модуль, содержащий интерфейс для самых популярных алгоритмов хеширования. 
```Python
import hashlib
# для списка доступных алгоритмов используются
print(hashlib.algorithms_available)
print(hashlib.algorithms_guaranteed)

import hashlib

mystring = input('Enter String to hash: ')
# если вам нужно принять какой-то ввод с консоли и хешировать его, закодировать строку в последовательности байтов: Предположительно по умолчанию UTF-8
hash_object = hashlib.md5(mystring.encode())# алгоритм хеширования md5  

hash_object = hashlib.sha1(b'Hello World') 	# алгоритм хеширования SHA1 
print(hash_object.hexdigest())
```
#### Реальный пример хеширования паролей
```py
import uuid
import hashlib
 
def hash_password(password):
    # uuid используется для генерации случайного числа
    salt = uuid.uuid4().hex
    return hashlib.sha256(salt.encode() + password.encode()).hexdigest() + ':' + salt
    
def check_password(hashed_password, user_password):
    password, salt = hashed_password.split(':')
    return password == hashlib.sha256(salt.encode() + user_password.encode()).hexdigest()
 
new_pass = input('Введите пароль: ')
hashed_password = hash_password(new_pass)
print('Строка для хранения в базе данных: ' + hashed_password)
old_pass = input('Введите пароль еще раз для проверки: ')

if check_password(hashed_password, old_pass):
    print('Вы ввели правильный пароль')
else:
    print('Извините, но пароли не совпадают')
```

Числовые значения, которые при сравнении являются равными, имеют одинаковое значение хеш-функции (даже если они имеют разные типы, как в случае с 1, 1.0 или True)

_____________
#### Links
https://python-scripts.com/md5-sha1
https://codechick.io/tutorials/dsa/dsa-hash-table