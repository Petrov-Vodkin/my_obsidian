2021-02-04 14:59
#безокон 
# PhantomJS
Если вам необходимо взаимодействовать с сайтом, или запарсить информацию, **но не скачивать файлы**, то хорошим вариантом будет PhantomJS. Всё что вам нужно это просто запустить его, автор создал его как раз для работы в строке, и поэтому никаких режимов запуска к нему приписывать не нужно, вот пример:

```python
from selenium import webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

ua = dict(DesiredCapabilities.PHANTOMJS)
ua["phantomjs.page.settings.userAgent"] = ("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.78 Safari/537.36")
browser = webdriver.PhantomJS(desired_capabilities=ua)
browser.set_window_size(1920, 1080)
browser.get('https://www.google.com/')
```
_____________
#### Links
[[Selenium]] [[Python]]