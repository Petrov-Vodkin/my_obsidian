2021-01-20 19:38
#фичи #буферобмена
# Selenium сниппеты
#### Копирование числа из алерта в буфер обмена  
``` python
alert = browser.switch_to.alert  
alert_text = alert.text  
addToClipBoard = alert_text.split(': ')[-1]  
pyperclip.copy(addToClipBoard)
```
#### async fashion [](https://stackoverflow.com/questions/50303797/python-webdriver-and-asyncio)
```python
import asyncio
from concurrent.futures.thread import ThreadPoolExecutor
from selenium import webdriver

executor = ThreadPoolExecutor(10)

def scrape(url, *, loop):
    loop.run_in_executor(executor, scraper, url)

def scraper(url):
    driver = webdriver.Chrome("./chromedriver")
    driver.get(url)

loop = asyncio.get_event_loop()
for url in ["https://google.de"] * 2:
    scrape(url, loop=loop)

loop.run_until_complete(asyncio.gather(*asyncio.all_tasks(loop)))
```
#### Links
[[Selenium]] 
[многопоточный веб-парсер с Python и Selenium](https://www.awesomeandrew.ru/2020/12/06/%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%B5%D0%BC-%D0%BC%D0%BD%D0%BE%D0%B3%D0%BE%D0%BF%D0%BE%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9-%D0%B2%D0%B5%D0%B1-%D0%BF%D0%B0%D1%80%D1%81%D0%B5%D1%80-%D1%81-python-%D0%B8-s/)