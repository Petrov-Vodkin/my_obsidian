2020-12-08 20:00
#ожидание #waits
# Selenium Waits

### Явное ожидание
это код, которым вы определяете какое необходимое условие должно произойти для того, чтобы дальнейший код исполнился. Худший пример такого кода — это использование команды time.sleep(), которая устанавливает точное время ожидания. WebDriverWait в комбинации с ExpectedCondition является одним из таких способов.
``` python
	from selenium import webdriver
	from selenium.webdriver.common.by import By
	from selenium.webdriver.support.ui import WebDriverWait
	from selenium.webdriver.support import expected_conditions as EC

	driver = webdriver.Firefox()
	driver.get("http://somedomain/url_that_delays_loading")
	try:
		element = WebDriverWait(driver, 10).until(
			EC.presence_of_element_located((By.ID, "myDynamicElement"))
		)
	finally:
		driver.quit()
```

Этот код будет ждать 10 секунд до того, как отдаст исключение TimeoutException или если найдет элемент за эти 10 секунд, то вернет его. WebDriverWait по умолчанию вызывает ExpectedCondition каждые 500 миллисекунд до тех пор, пока не получит успешный return. Успешный return для ExpectedCondition имеет тип Boolean и возвращает значение true, либо возвращает not null для всех других ExpectedCondition типов.

------------------------
 #### [Ожидаемые условия](https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.support.expected_conditions)
 Существуют некие условия, которые часто встречаются при автоматизации веб-сайтов. Ниже перечислены реализации каждого. Связки в Selenium Python предоставляют некоторые удобные методы, так что вам не придется писать класс expected_condition самостоятельно или же создавать собственный пакет утилит.

*    title_is
*    title_contains
*    presence_of_element_located
*    visibility_of_element_located
*    visibility_of
*    presence_of_all_elements_located
*    text_to_be_present_in_element
*    text_to_be_present_in_element_value
*    frame_to_be_available_and_switch_to_it
*    invisibility_of_element_located
*    element_to_be_clickable — it is Displayed and Enabled.
*    staleness_of
*    element_to_be_selected
*    element_located_to_be_selected
*    element_selection_state_to_be
*    element_located_selection_state_to_be
*    alert_is_present

``` python
from selenium.webdriver.support import expected_conditions as EC
wait = WebDriverWait(driver, 10)
element = wait.until(EC.element_to_be_clickable((By.ID,'someid')))
```

Модуль expected_conditions уже содержит набор предопределенных условий для работы с WebDriverWait.

### Неявные ожидания

Неявное ожидание указывает WebDriver'у опрашивать DOM определенное количество времени, когда пытается найти элемент или элементы, которые недоступны в тот момент. Значение по умолчанию равно 0. После установки, неявное ожидание устанавливается для жизни экземпляра WebDriver объекта.
``` python
from selenium import webdriver
driver = webdriver.Firefox()
driver.implicitly_wait(10) # seconds
driver.get("http://somedomain/url_that_delays_loading")
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```

Перейти к следующей главе: [Объекты Страницы](http://habrahabr.ru/post/273115/)

#### Links
https://habr.com/ru/post/273115/
[[Selenium]] 