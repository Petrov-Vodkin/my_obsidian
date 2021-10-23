2021-08-03 13:57
#tag
# aiogram+sql 
Сначала установим aiogram  
`pip install -U aiogram`  
Начнем настройку бота!  
```python
from aiogram import Bot, types
from aiogram.dispatcher import Dispatcher
from aiogram.utils import executor


bot = Bot(token='token')
dp = Dispatcher(bot)
```
Вот и первое отличие от telebot, обработчиком выступает dp, а не bot, как мы привыкли  
Теперь делаем реакцию на /start  
```python
@dp.message_handler(commands=['start'])
async def process_start_command(message: types.Message):
    await message.reply('Привет, я эхо-бот, напиши мне что-нибудь!')
```
Здесь мы "отвечаем" на сообщение пользователя!  
Вы сейчас заметили, что перед def написано async  
async участвует в определении асинхронной функции  
Теперь напишем реакцию на сообщения:  
```python
@dp.message_handler()
async def echo_message(msg: types.Message):
    await bot.send_message(msg.from_user.id, msg.text)
```
Думаю, тут все понятно.  
Теперь последний штрих:  
```python
if __name__ == '__main__':
    executor.start_polling(dp)
```
Для тех, кто не понимает, зачем здесь if __name == '__main__':  
Он тут нужен для проверки того, что файл - запустили, а если его запустили, то он выполняет executor.start_polling(dp)  
executor.start_polling(dp) - как bot.polling() в telebot  
Все, вот целый код:  
```python
from aiogram import Bot, types
from aiogram.dispatcher import Dispatcher
from aiogram.utils import executor

bot = Bot(token='1175627112:AAFoCWMtbPtA7KZfdd45vUGXlA0Ss1wd0-o')
dp = Dispatcher(bot)


@dp.message_handler(commands=['start'])
async def process_start_command(message: types.Message):
    await message.reply('Привет, я эхо-бот, напиши мне что-нибудь!')


@dp.message_handler()
async def echo_message(msg: types.Message):
    await bot.send_message(msg.from_user.id, msg.text)


if __name__ == '__main__':
    executor.start_polling(dp)
```

Все! Мы написали с вами Эхо-бот!  
Так-с, теперь БД!  
Начнем с начала:  
**sqlite3 - встроенная библиотека, к установке не требуется**  

Python:

```python
import sqlite3

#conn = sqlite3.connect('имя_базы_данных.расширение(.db,.sqlite3)')
conn = sqlite3.connect('data.db')
cur = conn.cursor('')
```

conn - создание подключения  
cur - нужен для взаимодействия с БД  
Если такой базы данных нету, то он ее создаст  
Если бд находиться в папке, то можно написать так:  
`conn = sqlite3.connect('./название_папки/имя_базы_данных.расширение(.db,.sqlite3)')`  
WARNING!!!  
Папка должна находиться рядом с исполняемым файлом.  
Теперь надо создать таблицы в БД:  

Python:

```python
import sqlite3

conn = sqlite3.connect('data.db')
cur = conn.cursor()
cur.execute('CREATE TABLE users(user_id INTEGER, username TEXT)')
```

Все, таким образом мы создаем таблицы в БД  
cur.execute - Действия в БД(**Командами!**)  
А вот и данные, которые мы будем заносить!  
Теперь добавим в наш код добавление пользователя в БД и получение информации о себе через команду:  

Python:

```python
from aiogram import Bot, types
from aiogram.dispatcher import Dispatcher
from aiogram.utils import executor
import sqlite3

bot = Bot(token='token')
dp = Dispatcher(bot)


@dp.message_handler(commands=['start'])
async def process_start_command(message: types.Message):
    await message.reply('Привет, я эхо-бот, напиши мне что-нибудь!')
    try:
        conn = sqlite3.connect('data.db')
        cur = conn.cursor()
        cur.execute(f'INSERT INTO users VALUES("{message.from_user.id}", "@{message.from_user.username}")')
        conn.commit()
    except Exception as e:
        print(e)
        conn = sqlite3.connect('data.db')
        cur = conn.cursor()
        cur.execute(f'INSERT INTO users VALUES("{message.from_user.id}")')
        conn.commit()
```

Здесь мы просто добавляем значения id и username в БД, а если выдает ошибку, то только id  
conn.commit() - сохранение изменений  
Теперь настроим вывод по команде /profile:  

Python:

```python
@dp.message_handler(commands=['profile'])
async def get_profile(msg: types.Message):
    conn = sqlite3.connect('data.db')
    cur = conn.cursor()
    cur.execute(f'SELECT * FROM users WHERE user_id = "{msg.from_user.id}"')
    result = cur.fetchall()
    await bot.send_message(msg.from_user.id, f'ID = {list(result[0])[0]}\nUserName = {[list(result[0])[1]][0]}')
    #Да, я художник, я так вижу, а кто шарит, может мне подсказать, буду благодарен
```

Здесь мы выбираем все значения из таблицы users по id пользователя  
result = cur.fetchall() - сохранение полученных данных, в виде: [(первые данные, вторые данные)]  
Полный код:  

Python:

```python
from aiogram import Bot, types
from aiogram.dispatcher import Dispatcher
from aiogram.utils import executor
import sqlite3

bot = Bot(token='token')
dp = Dispatcher(bot)


@dp.message_handler(commands=['start'])
async def process_start_command(message: types.Message):
    await message.reply('Привет, я эхо-бот, напиши мне что-нибудь!')
    try:
        conn = sqlite3.connect('data.db')
        cur = conn.cursor()
        cur.execute(f'INSERT INTO users VALUES("{message.from_user.id}", "@{message.from_user.username}")')
        conn.commit()
    except Exception as e:
        print(e)
        conn = sqlite3.connect('data.db')
        cur = conn.cursor()
        cur.execute(f'INSERT INTO users VALUES("{message.from_user.id}")')
        conn.commit()

@dp.message_handler(commands=['profile'])
async def get_profile(msg: types.Message):
    conn = sqlite3.connect('data.db')
    cur = conn.cursor()
    cur.execute(f'SELECT * FROM users WHERE user_id = "{msg.from_user.id}"')
    result = cur.fetchall()
    print(result)
    await bot.send_message(msg.from_user.id, f'ID = {list(result[0])[0]}\nUserName = {[list(result[0])[1]][0]}')


@dp.message_handler()
async def echo_message(msg: types.Message):
    await bot.send_message(msg.from_user.id, msg.text)


if __name__ == '__main__':
    executor.start_polling(dp)
```
_____________
#### Links
