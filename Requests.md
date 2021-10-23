2021-03-06 19:00
#tag
# Requests
[Продвинутое руководство по библиотеке Python Requests](https://pythonru.com/biblioteki/prodvinutoe-rukovodstvo-po-biblioteke-python-requests)
[python-requests.org](https://docs.python-requests.org/en/master/api/#request-sessions)
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
_____________
#### Links
[[Python]] 
[smartiqa.ru/blog](https://smartiqa.ru/blog/python-requests)