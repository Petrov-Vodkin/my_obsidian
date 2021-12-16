2021-03-06 19:00
#tag
# Requests
[Краткое руководство по библиотеке Python Requests](https://pythonru.com/biblioteki/kratkoe-rukovodstvo-po-biblioteke-python-requests)
[Продвинутое руководство по библиотеке Python Requests](https://pythonru.com/biblioteki/prodvinutoe-rukovodstvo-po-biblioteke-python-requests)
[python-requests.org](https://docs.python-requests.org/en/master/api/#request-sessions)
#### Объекты Session
Объект `Session` позволяет сохранять определенные параметры между запросами. Он же сохраняет куки всех запросов, сделанных из экземпляра `Session`.

#### Кодировка
```python
import requests

url = 'symple.com'
r = requests.get(url)
# print(r.text)
print(r.text.encode('utf-8'))
# or
r.encoding = 'utf-8'  # указываем правильную кодировку принудительно
print(r.text)
# or
 print "Привет".decode('utf-8')
```
#### [[Proxy]]
```py
import requests
user_agent = {'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                            'Chrome/89.0.4389.72 Safari/537.36'}
proxy = {'http': 'http://185.253.98.21:3128',
     	'https': 'http://185.253.98.21:3128'}
response = requests.get('https://ramziv.com/ip')
response.text
# 29.174.182.126
session = requests.Session()
session.proxies.update(proxy)
session.headers.update(user_agent)
response = session.get('https://ramziv.com/ip')
response.text
# 185.253.98.21
```

#### Тайм-ауты
чтобы Requests прекратил ожидание ответа после определенного количества секунд
`Почти весь код должен использовать этот параметр в запросах`. Несоблюдение этого требования может привести к зависанию вашей программы:
```python
requests.get('https://github.com/', timeout=0.001)
"""
Timeout это не ограничение по времени полной загрузки ответа. 
Исключение возникает, если сервер не дал ответ за timeout секунд 
(точнее, если ни одного байта не было получено от основного сокета за timeout секунд).
"""
```
#### Потоковые загрузки
Requests поддерживает потоковые загрузки, которые позволяют отправлять крупные потоки или файлы без их чтения прямо в память. Для этого нужно предоставить [файловый объект](https://pythonru.com/osnovy/fajly-v-python-vvod-vyvod) в `data`:
```py
with open('massive-body') as f:
    request.post('http://some.url/streamed', data=f)
```
_____________
#### Links
[[Python]] 
[smartiqa.ru/blog](https://smartiqa.ru/blog/python-requests)