2022-02-27 21:21
[link](https://gadjimuradov.ru/post/python-httpx-http-klient-novogo-pokoleniya-dlya-python/)
# HTTPX
позволяет реализовывать синхронные и асинхронные HTTP-запросы, поддерживает HTTP1 и HTTP2
```python
import httpx


r = httpx.get('http://gadjimuradov.ru')
r_post = httpx.post('https://httpbin.org/post', data={'key': 'value'})
r_put = httpx.put('https://httpbin.org/put', data={'key': 'value'})
r_delete = httpx.delete('https://httpbin.org/delete')
r_ head= httpx.head('https://httpbin.org/get')
r_options = httpx.options('https://httpbin.org/get')

print(r.status_code)	# Выводим статус ответа 
print(r.headers)		# Выводим заголовки
print(r.text)			# Выводим ответ
```
### Перехват исключений
вызываемый сервер недоступен , или вы укажете неверный запрос или другие ошибки
базовым классом , от которого наследуются классы **`RequestError`** и **`HTTPStatusError`** является класс **`HTTPError`**
```python
try:
    response = httpx.get("https://gadjimuradov.ru")
    response.raise_for_status()
except httpx.RequestError as exc:
    print(f"Произошла ошибка при запросе {exc.request.url!r}.")
except httpx.HTTPStatusError as exc:
    print(f"Ошибочный код запроса {exc.response.status_code} при выполнени запроса {exc.request.url!r}.")
```
### Поддержка асинхронных запросов
```python
import httpx

async with httpx.AsyncClient() as client:
     r = await client.get(url)
```
### Поддержка HTTP/2 
HTTP/2 используется двоичный формат |  __```pip install httpx[http2]```__
HTTP/2 позволяет одному потоку TCP обрабатывать несколько одновременных запросов
```python
import httpx

async with httpx.AsyncClient(http2=True) as client:
    r = await client.get(url)
```
_____________
#### Links
[[Python WEB]]
