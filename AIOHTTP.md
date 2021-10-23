2021-03-17 15:50
#aiohttp #multiprocess #async
# AIOHTTP
выполнение асинхронных HTTP-запросов
[En](https://docs.aiohttp.org/en/stable/#welcome-to-aiohttp "Permalink to this headline")_ . _ _ . [Ru](https://pythonist.ru/rukovodstvo-po-sozdaniyu-api-zaprosov-v-python/)

### Getting Started[](https://docs.aiohttp.org/en/latest/index.html#getting-started "Permalink to this headline")
#### Client example[](https://docs.aiohttp.org/en/latest/index.html#client-example "Permalink to this headline")
```py
import aiohttp
import asyncio

async def main():

    async with aiohttp.ClientSession() as session:
        async with session.get('http://python.org') as response:

            print("Status:", response.status)
            print("Content-type:", response.headers['content-type'])

            html = await response.text()
            print("Body:", html[:15], "...")

loop = asyncio.get_event_loop()
loop.run_until_complete(main())

#Status: 200
#Content-type: text/html; charset=utf-8
#Body: <!doctype html> ...
```

Coming from [requests](https://docs.aiohttp.org/en/latest/glossary.html#term-requests) ? Read [why we need so many lines](https://docs.aiohttp.org/en/latest/http_request_lifecycle.html#aiohttp-request-lifecycle).

#### Server example:[](https://docs.aiohttp.org/en/latest/index.html#server-example "Permalink to this headline")
```py
from aiohttp import web

async def handle(request):
    name = request.match_info.get('name', "Anonymous")
    text = "Hello, " + name
    return web.Response(text=text)

app = web.Application()
app.add_routes([web.get('/', handle),
                web.get('/{name}', handle)])

if __name__ == '__main__':
    web.run_app(app)
```
For more information please visit [Client](https://docs.aiohttp.org/en/latest/client.html#aiohttp-client) and [Server](https://docs.aiohttp.org/en/latest/web.html#aiohttp-web) pages.

##### асинхронные веб-запросы с aiohttp

В следующем примере показано, как можно реализовать это высокоуровневое асинхронное API. Ниже приведена измененная версия, обновленная для Python 3.7, примера Asyncio Скотта Робинсона ([Scott Robinson’s nifty asyncio](https://stackabuse.com/python-async-await-tutorial/)). Его программа использует модуль aiohttp для захвата верхних постов в Reddit и вывода их на консоль.

Убедитесь, что у вас установлен модуль **aiohttp**, прежде чем запускать скрипт ниже. Вы можете установить модуль с помощью следующей команды pip:

$ pip install --user aiohttp
```py
import sys  
import asyncio  
import aiohttp  
import json  
import datetime

async def get_json(client, url):  
    async with client.get(url) as response:
        assert response.status == 200
        return await response.read()

async def get_reddit_top(subreddit, client, numposts):  
    data = await get_json(client, 'https://www.reddit.com/r/' + 
        subreddit + '/top.json?sort=top&t=day&limit=' +
        str(numposts))

    print(f'\n/r/{subreddit}:')

    j = json.loads(data.decode('utf-8'))
    for i in j['data']['children']:
        score = i['data']['score']
        title = i['data']['title']
        link = i['data']['url']
        print('\t' + str(score) + ': ' + title + '\n\t\t(' + link + ')')

async def main():  
    print(datetime.datetime.now().strftime("%A, %B %d, %I:%M %p"))
    print('---------------------------')
    loop = asyncio.get_running_loop()  
    async with aiohttp.ClientSession(loop=loop) as client:
        await asyncio.gather(
            get_reddit_top('python', client, 3),
            get_reddit_top('programming', client, 4),
            get_reddit_top('asyncio', client, 2),
            get_reddit_top('dailyprogrammer', client, 1)
            )

asyncio.run(main())
```
Если вы запустите программу несколько раз, вы увидите, что порядок вывода изменится. Это связано с тем, что запросы JSON отображаются по мере их поступления, что зависит от времени ответа сервера и промежуточной задержки в сети. В системе Linux вы можете наблюдать это в действии, запустив скрипт с префиксом (например, watch -n 5), который будет обновлять вывод каждые 5 секунд:
_____________
#### Links
[[Библиотека Python]] [[Контекстный менеджер]]
https://webdevblog.ru/obzor-async-io-v-python-3-7/