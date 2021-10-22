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
async def cmd_test1(message: types.Message):
    await message.reply("Test 1")

if __name__ == "__main__":
    # Запуск бота
    executor.start_polling(dp, skip_updates=True)
```
Первое, на что нужно обратить внимание: aiogram — асинхронная библиотека, поэтому ваши функции тоже должны быть асинхронными, а перед вызовами методов API нужно ставить ключевое слово **await**, т.к. эти вызовы возвращают [корутины](https://docs.python.org/3/library/asyncio-task.html#coroutines).
-   [Знакомство с aiogram](https://mastergroosha.github.io/telegram-tutorial-2/quickstart/)
-   [Работа с собщениями](https://mastergroosha.github.io/telegram-tutorial-2/messages/)
-   [Кнопки](https://mastergroosha.github.io/telegram-tutorial-2/buttons/)
-   [Конечные автоматы](https://mastergroosha.github.io/telegram-tutorial-2/fsm/)
-   [Инлайн-режим](https://mastergroosha.github.io/telegram-tutorial-2/inline_mode/)
_____________
#### Links
[Official aiogram Docs](https://docs.aiogram.dev/en/latest/)