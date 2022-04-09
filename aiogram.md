2021-08-03 13:31
#tag `pip install aiogram` [[aiogram+sqlite3]]
# aiogram
### [Первый бот](https://mastergroosha.github.io/telegram-tutorial-2/quickstart/#_2 "Permanent link")
Давайте создадим файл `bot.py` с базовым шаблоном бота на aiogram:
```py
#!venv/bin/python
import logging
from aiogram import Bot, Dispatcher, executor, types

# Объект бота
bot = Bot(token="12345678:AaBbCcDdEeFfGgHh")
# Диспетчер для бота
dp = Dispatcher(bot)
# Включаем логирование, чтобы не пропустить важные сообщения
logging.basicConfig(level=logging.INFO)

# Хэндлер на команду /test1: ввод /test1 запустит функ-ю cmd_test1
@dp.message_handler(commands="test1") # зарегистрировать функцию как обработчик сообщений
async def cmd_test1(message: types.Message): # аннотация message: types.Message
    await message.reply("Test 1")

if __name__ == "__main__":
    # Запуск бота
    executor.start_polling(dp, skip_updates=True)
```
Первое, на что нужно обратить внимание: aiogram — асинхронная библиотека, поэтому ваши функции тоже должны быть асинхронными, а перед вызовами методов API нужно ставить ключевое слово **await**, т.к. эти вызовы возвращают [корутины](https://docs.python.org/3/library/asyncio-task.html#coroutines).
```python
								"""отправка видео"""
@dp.message_handler(commands=['test'])
async def cmd_image(message: types.Message):
    with open('/storage/emulated/0/Movies/3e36b80b873e29689147792373da5934.mp4', 'rb') as video:
        await message.answer_video(video)
```

### Проэкт бота[(модульный)](https://geekstand.top/development/aiogram-urok-2-navodim-porjadki-i-dobavljaem-filtry/) `AIOGram | Python`
[Урок 1. Вступление. Простой бот](https://geekstand.top/development/aiogram-urok-1-vstuplenie-prostoj-bot-na-aiogram/)
[Урок 2](https://geekstand.top/development/aiogram-urok-2-navodim-porjadki-i-dobavljaem-filtry/) зарделение проэкта на пакеты

### Бот + docker + деплой на aws
1.  [Настройка](https://tproger.ru/articles/telegram-bot-create-and-deploy/#part1)
2.  [Hello, bot!](https://tproger.ru/articles/telegram-bot-create-and-deploy/#part2)
3.  [Docker](https://tproger.ru/articles/telegram-bot-create-and-deploy/#part3)
4.  [Деплой на AWS](https://tproger.ru/articles/telegram-bot-create-and-deploy/#part4)
5.  [Заключение](https://tproger.ru/articles/telegram-bot-create-and-deploy/#part5)



_____________
#### Links
[Курс на Udemy](https://www.udemy.com/course/aiogram-python/)
https://surik00.gitbooks.io/aiogram-lessons/content/chapter1.html
-   [Титульный лист](https://surik00.gitbooks.io/aiogram-lessons/content/)
-   [Урок 1. Быстрый старт. Эхо-бот](https://surik00.gitbooks.io/aiogram-lessons/content/chapter1.html)
-   [Урок 2. Медиа, разметка, эмоджи и щепотка логирования](https://surik00.gitbooks.io/aiogram-lessons/content/chapter2.html)
-   [Урок 3. Машина состояний и то самое логгирование](https://surik00.gitbooks.io/aiogram-lessons/content/chapter3.html)
-   [Урок 4. Платежи в Telegram](https://surik00.gitbooks.io/aiogram-lessons/content/chapter4.html)
-   [Урок 5. Клавиатуры и кнопки](https://surik00.gitbooks.io/aiogram-lessons/content/chapter5.html)

[Official telegram bots api](https://core.telegram.org/bots/api)
[Official aiogram Docs](https://docs.aiogram.dev/en/latest/)
-   [Знакомство с aiogram](https://mastergroosha.github.io/telegram-tutorial-2/quickstart/)
-   [Работа с собщениями](https://mastergroosha.github.io/telegram-tutorial-2/messages/)
-   [Кнопки](https://mastergroosha.github.io/telegram-tutorial-2/buttons/)
-   [Конечные автоматы](https://mastergroosha.github.io/telegram-tutorial-2/fsm/)
-   [Инлайн-режим](https://mastergroosha.github.io/telegram-tutorial-2/inline_mode/)