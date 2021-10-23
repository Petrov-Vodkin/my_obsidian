2021-03-10 15:03
#proxy
# Proxy 
```python
# формат записи
proxies = {
    'http': 'xxx.xx.xxx.xx:xxxx',
    'https': 'xxx.xx.xxx.xx:xxxx'
}
```
```python
# перебор прокси в цикле
proxies = []
for proxy in proxies:
    response = requests.get(proxies=proxy)
    if response.status_code == requests.codes['ok']:
        break

response.text
```
```python
# proxy с авторизацией
import requests
from requests.auth import HTTPProxyAuth

url = 'http://google.com/'
proxies = {'http': '207.164.21.34:3128'}
auth = HTTPProxyAuth('my_login', 'my_password')

response = requests.get(url=url, proxies=proxies, auth=auth)
response.close()

print(response.status_code) # 200 - good
```
_____________
#### Links
[[Requests]] [[Python]]