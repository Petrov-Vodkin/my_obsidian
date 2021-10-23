2021-01-22 14:07
#tag
# [Отрицательные проверки](https://stepik.org/lesson/201964/step/5?unit=176022)
Общий совет — использовать явные ожидания и [Expected Conditions](https://selenium-python.readthedocs.io/waits.html), о которых мы говорили в предыдущих модулях.
 -------------------------
 **1. Элемент потенциально может появится на странице** (но вообще-то не должен).
 Добавить в BasePage абстрактный метод, который проверяет, что элемент не появляется на странице в течение заданного времени: 
``` python
def is_not_element_present(self, how, what, timeout=4):
    try:
        WebDriverWait(self.browser, timeout).until(EC.presence_of_element_located((how, what)))
    except TimeoutException:
        return True

    return False
```
Тогда его использование Page Object для страницы товара будет выглядеть так: 
``` python
def should_not_be_success_message(self):
    assert self.is_not_element_present(*ProductPageLocators.SUCCESS_MESSAGE), \
       "Success message is presented, but should not be"
```
-------------------------------------- 
**2. Элемент присутствует на странице и должен исчезнуть** со временем или в результате действий пользователя. Например, удаление товара из корзины, или исчезновение лоадера с загрузкой. 
Явным ожиданием вместе с функцией until\_not, в зависимости от того, какой результат мы ожидаем: 
``` python
def is_disappeared(self, how, what, timeout=4):
    try:
        WebDriverWait(self.browser, timeout, 1, TimeoutException).until_not(EC.presence_of_element_located((how, what))) 
# частоту опроса - WebDriver ждёт 4 секунды(timeout) и делает запросы каждую секунду до появления TimeoutException
    except TimeoutException:
        return False

    return True
```



#### Links
[[PyTest]] [[Python]] 
