2021-10-29 04:40
#tag
# EventLoop
#### Корутины
```py
# вывести состояние генератора
from inspect import getgeneratorstate  
getgeneratorstate(generator)
#__________________________________________________________________________

def average():  				# среднее арифметическое
    count, summ, average = 0, 0, None  
 while 1:  
        try:  
            x = yield average  	# отдаёт average, принемает x
        except StopIteration:  
            print('Done')  
        else:  
            count += 1  
 			summ += x  
            average = round(summ/count, 2)  
  
  
gener = average()  		# передаём переменной ссылку на генератор average
next(gener)  			# первый прогон (м заменить на gener.send(None))
print(gener.send(4))  	# 0+4/1 = 4
print(gener.send(5))	# 4+5/2 = 4.5


```
#### Async На генераторах
```py
import socket  
from select import select  
# select мониторит\показывает какой сокет готов  
  
  
tasks = [] # очерередь  
  
to_read, to_write = {}, {}  # (ключ - сокет, значение - генератор)  
  
  
def server():  
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)  
    server_socket.bind(('127.0.0.1', 5001))  
    server_socket.listen()  
  
    while 1:  
        yield server_socket  
        client_socket, addr = server_socket.accept() # read(ждёт\читает из буфера)  
 print('Connection from', addr)  
        tasks.append(client(client_socket))  
  
  
def client(client_socket):  
    while 1:  
        yield ('read', client_socket) # помечаем как read  
 request = client_socket(4096)   # read  
  
 if not request:  
            break  
 else:  
            response = "Hello world\n".encode()  
            yield ('write', client_socket)  # помечаем как whrite  
 client_socket.send(response)     # write (пишет в буфер)  
  
 client_socket.close()  
  
  
def event_loop():  
    # если любой(хоть какой-нить) из списков не пустой,то any вернёт true, иначе false  
 while any([tasks, to_read, to_write]):  
        while not tasks: # если tasks пустой  
 ready_to_read, ready_to_write, _ = select(to_read, to_write, []) # предаёт то, будем отслеживать  
  
 for sock in ready_to_read:  
                # наполняем список tasks новыми заданиями извлекая их из to_read (в нём они удалятся  
 tasks.append(to_read.pop(sock))  
  
            for sock in ready_to_write:  
                tasks.append(to_read.pop(sock))  
  
        try:  
            task = tasks.pop(0)  
            reason, sock = next(task)  
            if reason == 'read':  
                to_read[sock] = task  
            if reason == 'write':  
                to_write[sock] = task  
        except StopIteration:  
            print('Done')  
  
  
tasks.append(server())  
event_loop()
```
#### Генераторы и событийный цикл Round Robin
генератор - функия
```py
from time import time  
  
  
def gen_of_str(s):  
    for i in s:  
        yield i  
  
  
def gen_of_num(s):  
    for i in range(s):  
        yield i  
  
  
g_s = gen_of_str('any string')  
g_n = gen_of_num(11)  
  
tasks = [g_s, g_n] # список с генераторами  
  
while tasks:      # пока список(tasts) содержит генераторы  
	task = tasks.pop(0)  #!!!!!!!  получаем первую задачу(==0) из списка _______________  
	try:  
        i = next(task)  
        print(i)        # здесь модет быть полезная задача  
		tasks.append(task) #!!!!!!! попытка вернуть генератор в список(ессли он не израсходовался)  
	except StopIteration:  
		pass # отлавливаем конец итерации(для продолжения скрипта)		
```

#### Модуль [select](https://docs-python.ru/standart-library/modul-select-python/funktsija-select-modulja-select/)  в Python
`Позволяет "отловить"` готовность объекта(|функции), принять, записать что-то или возможность ошибки.
Первые три аргумента функция модуля `select.select()` являются итерациями "ожидаемых объектов": либо целых чисел, представляющих файловые дескрипторы, либо объектов с методом без параметров с именем [`file.fileno()`](https://docs-python.ru/tutorial/metody-fajlovogo-obekta-potoka-python/metod-file-fileno/ "Метод file.fileno() в Python, получает файловый дескриптор."), возвращающим такое целое число:
-   `rlist`: список объектов, по готовности из которых нужно что-то прочитать,
-   `wlist`: список объектов, по готовности в которые нужно что-то записать,
-   `xlist`: список объектов, в которых возможно будут ошибки (что конкретная система считает ошибками можно посмотреть страницу руководства командой терминала `$ man select`)

#### Модуль [Selectors](https://docs-python.ru/standart-library/modul-selectors-python/) 
-обеспечивает высокоуровневое и эффективное мультиплексирование ввода-вывода, основанное на примитивах [модуля `select`](https://docs-python.ru/standart-library/modul-select-python/ "Модуль select в Python, отслеживание операций ввода/вывода.")
```py
import selectors
import socket

sel = selectors.DefaultSelector()

def accept(sock, mask):
    # Должен быть готов к чтению
    conn, addr = sock.accept()  
    print('Создан сокет:', conn, 'для адреса:', addr)
    # включаем для него неблокирующий режим
    conn.setblocking(False) 
    # регистрируем для прослушивания событий клиента
    sel.register(conn, selectors.EVENT_READ, read)

def read(conn, mask):
    # Должен быть готов к чтению
    data = conn.recv(1000)
    if data:
        print('Отвечаем: ', repr(data), 'сокету =>', conn)
        conn.send(data)
    else:
        print('Закрытие соединения', conn)
        sel.unregister(conn)
        conn.close()

sock = socket.socket()
sock.bind(('localhost', 8008))
sock.listen(5)
sock.setblocking(False)
sel.register(sock, selectors.EVENT_READ, accept)

while True:
    events = sel.select()
    for key, mask in events:
        callback = key.data
        callback(key.fileobj, mask)
```


_____________
#### Links
