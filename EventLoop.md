2021-10-29 04:40
#tag
# EventLoop
#### Функция [select() модуля select](https://docs-python.ru/standart-library/modul-select-python/funktsija-select-modulja-select/)  в Python
`Позволяет "отловить"` готовность объекта(|функции), принять, записать что-то или возможность ошибки.
Первые три аргумента функция модуля `select.select()` являются итерациями "ожидаемых объектов": либо целых чисел, представляющих файловые дескрипторы, либо объектов с методом без параметров с именем [`file.fileno()`](https://docs-python.ru/tutorial/metody-fajlovogo-obekta-potoka-python/metod-file-fileno/ "Метод file.fileno() в Python, получает файловый дескриптор."), возвращающим такое целое число:
-   `rlist`: список объектов, по готовности из которых нужно что-то прочитать,
-   `wlist`: список объектов, по готовности в которые нужно что-то записать,
-   `xlist`: список объектов, в которых возможно будут ошибки (что конкретная система считает ошибками можно посмотреть страницу руководства командой терминала `$ man select`)

#### Selectors
```py
import selectors
selectors.Defaultselector() 		# вывод деф сеоектора


selector = selectors.Defaultselector()

selector.reqiester(fileobj=server_socket...........)
```
_____________
#### Links
