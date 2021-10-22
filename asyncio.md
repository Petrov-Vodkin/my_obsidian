2021-10-17 12:18
[doc-py](https://docs-python.ru/standart-library/modul-asyncio-python/)
# asyncio
Есть определенный список правил, касающийся использования команд `async`/`await`.

-   Функция, которая начинается с [`async def` является сопрограммой](https://docs-python.ru/tutorial/sintaksis-async-await-python/soprogrammy-korutiny-async-def/ "Сопрограммы/корутины async def в Python.").
-   В теле сопрограммы можно использовать выражения `await`, [`return`](https://docs-python.ru/tutorial/opredelenie-funktsij-python/instruktsija-return/ "Инструкция return в Python") или [`yield`](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-generator-generator/ "Тип данных generator (генератор).").
-   Использование в сопрограмме ключевого слова `yield` создает [асинхронный генератор](https://docs-python.ru/tutorial/sintaksis-async-await-python/asinhronnyj-generator/ "Асинхронный генератор в Python."), через который можно итерироваться с помощью [`async for`](https://docs-python.ru/tutorial/sintaksis-async-await-python/asinhronnyj-async-for/ "Асинхронный async for в Python.").
-   Использование в сопрограмме выражения `async with` запускает асинхронные контекстные менеджеры.
-   Сопрограммы не могут использовать выражение [`yield from`](https://docs-python.ru/tutorial/generatory-python/vyrazhenie-yield-from-expr/ "Выражение yield from <expression> в Python."). Это вызовет [исключение `SyntaxError`](https://docs-python.ru/tutorial/vstroennye-iskljuchenija-interpretator-python/vstroennye-iskljuchenija/ "Исключения наследуемые от Exception в Python.").
-   Ключевое слово `await` можно использовать только в теле сопрограммы. Вызов `await` в другом месте спровоцирует исключение `SynatxError`.
-   Ключевое слово `await` требует наличия `awaitable` объекта. Такими объектами могут быть другая сопрограмма или объект у которого определен метод `__await__()`.

Ключевое слово `await` позволяет сопрограмме отдать контроль назад в главный цикл, который содержит порядок исполнения всех сопрограмм.
```py
async def process():
    result = await func()
    retiurn result
```
Если Python встречает ключевое слово `await` то это можно описать так: - "отложить исполнение кода сопрограммы `process()` до тех пор, пока я не получу результат выполнения `func()`. В это время я займусь чем-нибудь другим".

---
[Сопрограммы/корутины async def в Python.](https://docs-python.ru/tutorial/sintaksis-async-await-python/soprogrammy-korutiny-async-def/ "Асинхронные функции (сопрограммы) в Python.")
Внутри тела функции сопрограммы идентификаторы await и async становятся зарезервированными ключевыми словами. Выражения await, async for и async with могут использоваться только в телах функций сопрограмм.
[Асинхронный async for в Python.](https://docs-python.ru/tutorial/sintaksis-async-await-python/asinhronnyj-async-for/ "Асинхронный async for в Python.")
Асинхронный оператор `async for ... in` обеспечивает удобную итерацию по асинхронным итераторам и асинхронным генераторам. Асинхронный оператор `async for ... in` действует только в теле асинхронной функции (сопрограммы) `async def`.
[Асинхронный контекст-менеджер async with в Python.](https://docs-python.ru/tutorial/sintaksis-async-await-python/asinhronnyj-kontekst-menedzher-async-with/ "Асинхронный контекстный менеджер async with в Python.")
Асинхронный менеджер контекста может приостановить выполнение в своих методах `__aenter__()` и `__aexit__()`. Асинхронный оператор `with` может использоваться только в теле асинхронной функции (сопрограммы).
[Асинхронный итератор в Python.](https://docs-python.ru/tutorial/sintaksis-async-await-python/asinhronnyj-iterator/ "Асинхронный итератор aiterator в Python.")
Асинхронный итератор может вызывать асинхронный код в своем методе `__anext__` и могут использоваться только в асинхронном операторе `async for`.
[ Асинхронный генератор в Python.](https://docs-python.ru/tutorial/sintaksis-async-await-python/asinhronnyj-generator/ "Асинхронный генератор в Python.")
Наличие выражения yield в функции или методе, определенном с использованием async def, дополнительно определяет функцию как функцию асинхронного генератора.
### Сопрограммы
Сопрограммы (coroutine) — это обобщенные формы подпрограмм. Они используются для кооперативных задач и ведут себя как генераторы Python.
Для определения сопрограммы асинхронная функция использует ключевое слово `await`. При его использовании сопрограмма передает поток управления обратно в цикл событий (также известный как event loop).
Для запуска сопрограммы нужно запланировать его в цикле событий. После этого такие сопрограммы оборачиваются в задачи (`Tasks`) как объекты `Future`.
#### Пример сопрограммы
В коде ниже функция `async_func` вызывается из основной функции. Нужно добавить ключевое слово `await` при вызове синхронной функции. Функция `async_func` не будет делать ничего без `await`.
```python
import asyncio


async def async_func():
    print('Запуск ...')
    await asyncio.sleep(1)
    print('... Готово!')


async def main():
    async_func()  # этот код ничего не вернет 
    await async_func()


asyncio.run(main())
```
Вывод:
```
Warning (from warnings module):
  File "\AppData\Local\Programs\Python\Python38\main.py", line 8
    async_func() # этот код ничего не вернет
RuntimeWarning: coroutine 'async_func' was never awaited
Запуск ...
... Готово!
```
### Задачи (tasks)

Задачи используются для планирования параллельного выполнения сопрограмм.

При передаче сопрограммы в цикл событий для обработки можно получить объект Task, который предоставляет способ управления поведением сопрограммы извне цикла событий.

#### Пример задачи

В коде ниже создается `create_task` (встроенная функция библиотеки asyncio), после чего она запускается.

Копировать

```python
import asyncio


async def async_func():
    print('Запуск ...')
    await asyncio.sleep(1)
    print('... Готово!')


async def main():
    task = asyncio.create_task (async_func())
    await task

asyncio.run(main())
```

Вывод:

```
Запуск ...
... Готово!
```

### Циклы событий

Этот механизм выполняет сопрограммы до тех пор, пока те не завершатся. Это можно представить как цикл `while True`, который отслеживает сопрограммы, узнавая, когда те находятся в режиме ожидания, чтобы в этот момент выполнить что-нибудь другое.

Он может разбудить спящую сопрограмму в тот момент, когда она ожидает своего времени, чтобы выполниться. В одно время может выполняться лишь один цикл событий в Python.

#### Пример цикла событий

Дальше создаются три задачи, которые добавляются в список. Они выполняются асинхронно с помощью `get_event_loop`, `create_task` и await библиотеки asyncio.

Копировать

```python
import asyncio


async def async_func(task_no):
    print(f'{task_no}: Запуск ...')
    await asyncio.sleep(1)
    print(f'{task_no}: ... Готово!')


async def main():
    taskA = loop.create_task (async_func('taskA'))
    taskB = loop.create_task(async_func('taskB'))
    taskC = loop.create_task(async_func('taskC'))
    await asyncio.wait([taskA,taskB,taskC])


if __name__ == "__main__":
    try:
        loop = asyncio.get_event_loop()
        loop.run_until_complete(main())
    except :
        pass
```

Вывод:

```
taskA: Запуск ...
taskB: Запуск ...
taskC: Запуск ...
taskA: ... Готово!
taskB: ... Готово!
taskC: ... Готово!
```

### Future

`Future` — это специальный низкоуровневый объект, который представляет окончательный результат выполнения асинхронной операции.

Если этот объект подождать (`await`), то сопрограмма дождется, пока `Future` не будет выполнен в другом месте.

В следующих разделах посмотрим, на то, как `Future` используется.

  

## Сравнение многопоточности и асинхронности

Прежде чем переходить к асинхронности попробуем проверить многопоточность на производительность и сравним результаты. Для этого теста будем получать данные по URL с разной частотой: 1, 10, 50, 100 и 500 раз соответственно. После этого сравним производительность обоих подходов.

### Реализация

Многопоточность:

Копировать

```python
import requests
import time
from concurrent.futures import ProcessPoolExecutor


def fetch_url_data(pg_url):
    try:
        resp = requests.get(pg_url)
    except Exception as e:
        print(f"Возникла ошибка при получении данных из url: {pg_url}")
    else:
        return resp.content
        

def get_all_url_data(url_list):
    with ProcessPoolExecutor() as executor:
        resp = executor.map(fetch_url_data, url_list)
    return resp
    

if __name__=='__main__':
    url = "https://www.uefa.com/uefaeuro-2020/"
    for ntimes in [1,10,50,100,500]:
        start_time = time.time()
        responses = get_all_url_data([url] * ntimes)
        print(f'Получено {ntimes} результатов запроса за {time.time() - start_time} секунд')
```

Вывод:

```
Получено 1 результатов запроса за 0.9133939743041992 секунд
Получено 10 результатов запроса за 1.7160518169403076 секунд
Получено 50 результатов запроса за 3.842841625213623 секунд
Получено 100 результатов запроса за 7.662721633911133 секунд
Получено 500 результатов запроса за 32.575703620910645 секунд
```

`ProcessPoolExecutor` — это пакет Python, который реализовывает интерфейс `Executor`. `fetch_url_data` — функция для получения данных по URL с помощью библиотеки request. После получения `get_all_url_data` используется, чтобы замапить `function_url_data` на список URL.

Асинхронность:

Копировать

```python
import asyncio
import time
from aiohttp import ClientSession, ClientResponseError


async def fetch_url_data(session, url):
    try:
        async with session.get(url, timeout=60) as response:
            resp = await response.read()
    except Exception as e:
        print(e)
    else:
        return resp
    return


async def fetch_async(loop, r):
    url = "https://www.uefa.com/uefaeuro-2020/"
    tasks = []
    async with ClientSession() as session:
        for i in range(r):
            task = asyncio.ensure_future(fetch_url_data(session, url))
            tasks.append(task)
        responses = await asyncio.gather(*tasks)
    return responses


if __name__ == '__main__':
    for ntimes in [1, 10, 50, 100, 500]:
        start_time = time.time()
        loop = asyncio.get_event_loop()
        future = asyncio.ensure_future(fetch_async(loop, ntimes))
        # будет выполняться до тех пор, пока не завершится или не возникнет ошибка
        loop.run_until_complete(future)
        responses = future.result()
        print(f'Получено {ntimes} результатов запроса за {time.time() - start_time} секунд')
```

Вывод:

```
Получено 1 результатов запроса за 0.41477298736572266 секунд
Получено 10 результатов запроса за 0.46897053718566895 секунд
Получено 50 результатов запроса за 2.3057644367218018 секунд
Получено 100 результатов запроса за 4.6860511302948 секунд
Получено 500 результатов запроса за 18.013994455337524 секунд
```

Нужно использовать функцию `get_event_loop` для создания и добавления задач. Чтобы использовать более одного URL, нужно применить функцию `ensure_future`.

Функция `fetch_async` используется для добавления задачи в объект цикла событий, а `fetch_url_data` — для чтения данных URL с помощью пакета session. Метод `future_result` возвращает ответ всех задач.

### Результаты

Как можно увидеть, асинхронное программирование на порядок эффективнее многопоточности для этой программы.
_____________
#### Links