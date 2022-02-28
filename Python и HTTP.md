2021-09-18 10:00
[ru)blog](https://webdevblog.ru/python-i-http-klienty/)
# Python и HTTP
[[#Постоянные соединения]]
[[#Параллелизм]] | [[#Асинхронность]]
[[#Производительность]] | [[#Потоки]]

Оригинальная статья: [Julien Danjou](https://julien.danjou.info/author/jd/) — [Python and fast HTTP clients](https://julien.danjou.info/python-and-fast-http-clients/)

В Python есть много HTTP-клиентов (библиотек); наиболее широко используемый и простой в работа с [requests](http://docs.python-requests.org/). Это стандарт де-фактора в наши дни.

### Постоянные соединения

Первый способ, который необходимо принять во внимание, — это постоянное подключение к веб-серверу. Постоянные соединения являются стандартом начиная с HTTP 1.1, хотя многие приложения не используют их. Отсутствие оптимизации в нем легко объяснить, если вы знаете, что `при использовании запросов в простом режиме (например, с функцией `**get**`) соединение закрывается при получение ответа от сервера. Чтобы избежать этого`, приложению необходимо `использовать объект` **`Session`**, который позволяет `повторно использовать уже открытое соединение`.

**Использование сеанса (`Session`) с запросами**

```py
import requests

session = requests.Session()
session.get("http://example.com")
# Connection is re-used
session.get("http://example.com")
```
Каждое соединение хранится в пуле соединений (по умолчанию помещает 10 соединений), размер пула также настраивается:
**Изменение размера пула**
```py
import requests

session = requests.Session()
adapter = requests.adapters.HTTPAdapter(
pool_connections=100,
pool_maxsize=100)
session.mount('http://', adapter)
response = session.get("http://example.org")
```
Повторное использование TCP-соединения для отправки нескольких HTTP-запросов дает ряд преимуществ в производительности:

-   Снижение использования процессора и памяти (меньшее количество одновременно открытых соединений).
-   Уменьшенная задержка при последующих запросах (без TCP-handshaking).
-   Исключения могут быть подняты без штрафа закрытия TCP-соединения.

Протокол HTTP также обеспечивает конвейеризацию ([pipelining](https://en.wikipedia.org/wiki/HTTP_pipelining%5Bpipelining%5D)), которая позволяет отправлять несколько запросов по одному и тому же соединению, не дожидаясь получения ответов (думаю, пакет). К сожалению, это не поддерживается библиотекой **requests**. Однако конвейеризация запросов может быть не такой быстрой, как их параллельная отправка. Так как, протокол HTTP 1.1 заставляет отправлять ответы в том же порядке, в котором были отправлены запросы — первым пришел — первым вышел.

## Параллелизм

**requests** также имеют один существенный недостаток: эта библиотека синхронна. Вызов **request.get («http://example.org»)** блокирует программу до тех пор, пока HTTP-сервер не ответит полностью. Недостатком может быть то, что приложение во время запроса ожидает ответа и ничего не делает. Вполне возможно, что программа могла бы делать что-то еще, а не сидеть без дела.

Интеллектуальное приложение может смягчить эту проблему, используя пул потоков, подобных тем, которые предоставляются **concurrent.futures**. Это позволяет очень быстро распараллеливать HTTP-запросы.

Использование **futures** с **requests**
```py
from concurrent import futures
import requests

with futures.ThreadPoolExecutor(max_workers=4) as executor:
    futures = [
        executor.submit(
            lambda: requests.get("http://example.org"))
        for _ in range(8)
    ]

results = [
    f.result().status_code
    for f in futures
]

print("Results: %s" % results)
```
Этот шаблон довольно полезен, он был упакован в библиотеку [requests-futures](https://github.com/ross/requests-futures%5Brequests-futures%5D). С помощью его можно легко использовать объект **Session**:
```py
from requests_futures import sessions

session = sessions.FuturesSession()
futures = [
    session.get("http://example.org")
    for _ in range(8)
]

results = [
    f.result().status_code
    for f in futures
]
print("Results: %s" % results)
```
По умолчанию создается **worker** с двумя потоками, но программа может легко настроить это значение, передав аргумент **max_workers** или даже своего собственного исполнителя объекту **FuturSession** — например, так: **FuturesSession (executor = ThreadPoolExecutor (max_workers = 10))**.

## Асинхронность

Как объяснялось ранее, **requests** полностью синхронен. Он блокирует приложение в ожидании ответа сервера, замедляя работу программы. Создание HTTP-запросов в потоках является одним из решений, но потоки имеют свои собственные накладные расходы, и это подразумевает параллелизм, который не всегда каждый рад видеть в программе.

Начиная с версии 3.5, Python предлагает асинхронность внутри своего ядра, используя [aiohttp](http://aiohttp.readthedocs.io/%5Baiohttp%5D). Библиотека **aiohttp** предоставляет асинхронный HTTP-клиент, построенный поверх **asyncio**. Эта библиотека позволяет отправлять запросы последовательно, но не дожидаясь первого ответа, прежде чем отправлять новый. В отличие от конвейерной передачи HTTP, **aiohttp** отправляет запросы по нескольким соединениям параллельно, избегая проблемы, описанной ранее.

Использование [[AIOHTTP]]
```py
import aiohttp
import asyncio

async def get(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return response

loop = asyncio.get_event_loop()
coroutines = [get("http://example.com") for _ in range(8)]

results = loop.run_until_complete(asyncio.gather(*coroutines))
print("Results: %s" % results)
```
Все эти решения (с использованием **Session**, **thread**, **futures** или **asyncio**) предлагают разные подходы к ускорению работы HTTP-клиентов. Но какая между ними разница с точки зрения производительности?

## Производительность

Ниже приведен фрагмент HTTP-клиента, отправляющего запросы на **httpbin.org**, HTTP-API, который обеспечивает (среди прочего) конечную точку, имитирующую длинный запрос. Этот пример реализует все методы, перечисленные выше.

Программа для сравнения производительности использования различных запросов
```py
import contextlib
import time

import aiohttp
import asyncio
import requests
from requests_futures import sessions

URL = "http://httpbin.org/delay/1"
TRIES = 10


@contextlib.contextmanager
def report_time(test):
    t0 = time.time()
    yield
    print("Time needed for `%s' called: %.2fs"
          % (test, time.time() - t0))


with report_time("serialized"):
    for i in range(TRIES):
        requests.get(URL)


session = requests.Session()
with report_time("Session"):
    for i in range(TRIES):
        session.get(URL)


session = sessions.FuturesSession(max_workers=2)
with report_time("FuturesSession w/ 2 workers"):
    futures = [session.get(URL)
               for i in range(TRIES)]
    for f in futures:
        f.result()


session = sessions.FuturesSession(max_workers=TRIES)
with report_time("FuturesSession w/ max workers"):
    futures = [session.get(URL)
               for i in range(TRIES)]
    for f in futures:
        f.result()


async def get(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            await response.read()

loop = asyncio.get_event_loop()
with report_time("aiohttp"):
    loop.run_until_complete(
        asyncio.gather(*[get(URL)
                         for i in range(TRIES)]))
```
Запуск этой программы дает следующий вывод:
```shell
Time needed for `serialized' called: 12.12s
Time needed for `Session' called: 11.22s
Time needed for `FuturesSession w/ 2 workers' called: 5.65s
Time needed for `FuturesSession w/ max workers' called: 1.25s
Time needed for `aiohttp' called: 1.19s
```
![](https://julien.danjou.info/content/images/2019/07/20190716092338_hd.png)

Не удивительно, что более медленный результат приходит с сериализованной версией, поскольку все запросы выполняются один за другим без повторного использования соединения — 12 секунд на 10 запросов.

Использование объекта **Session** и, следовательно, повторное использование соединения означает экономию 8% времени, что уже является большим и легким выигрышем. Как минимум, вы всегда должны использовать **Session**.

Если ваша система и программа допускают использование потоков, рекомендуется использовать их для распараллеливания запросов. Однако у потоков есть некоторые накладные расходы, и они не менее весовые. Они должны быть созданы, запущены и затем присоединены.

Если вы не используете старые версии Python, то, без сомнения, использование **aiohttp** должно быть вашим выбором, если вы хотите написать быстрый и асинхронный HTTP-клиент. Это самое быстрое и масштабируемое решение, поскольку оно может обрабатывать сотни параллельных запросов.

## Потоки

Еще одна эффективная оптимизация скорости — это потоковая передача запросов. При отправке запроса по умолчанию все тело ответа загружается немедленно. Лучший способ не загружать весь контент в память сразу при запросе. Для этого есть параметра stream, в библиотеке **requests** или атрибут **content** в **aiohttp**.

Потоковая передача с **requests**

import requests
```py
import requests

# Use `with` to make sure the response stream is closed and the connection can
# be returned back to the pool.
with requests.get('http://example.org', stream=True) as r:
    print(list(r.iter_content()))
```
Потоковая передача с **aiohttp**
```py
import aiohttp
import asyncio


async def get(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.content.read()

loop = asyncio.get_event_loop()
tasks = [asyncio.ensure_future(get("http://example.com"))]
loop.run_until_complete(asyncio.wait(tasks))
print("Results: %s" % [task.result() for task in tasks])
```
Не загружать полный контент крайне важно, чтобы избежать ненужного выделения сотен мегабайт памяти. Если вашей программе не требуется доступ ко всему содержимому в целом, но она может работать с частями. Например, если вы собираетесь сохранить и записать содержимое в файл, чтение только куска и одновременная запись будет гораздо более эффективным, чем чтение всего тела HTTP, выделяя огромную кучу памяти , и только после этого записать его на диск.

## Заключение

Я надеюсь, это статья облегчит вам выбор правильного HTTP-клиента и написание запросов с его помощью. Если вы знаете какую-либо другую полезную технику или метод, не стесняйтесь опишите ее в разделе комментариев ниже (можно в блоге [автора](https://julien.danjou.info/python-and-fast-http-clients/) статьи)!
_____________
#### Links
https://webdevblog.ru/python-i-http-klienty/
[[Requests]]
