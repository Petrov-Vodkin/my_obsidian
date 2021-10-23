2021-05-04 13:55
#testing 
# Allure
`Allure Framework - инструмент для создания отчетов о тестировании`
### Установка
#### 0 Установка [[Scoop]] (не обязательно)
#### 1 Установка allure
- [Скачать](https://github.com/allure-framework/allure2/) последнюю версию allure–commandline
- Добавить путь до директории **bin** из распакованного архива в системную переменную окружения `PATH`
#### 2 Pip install 
```powershell
allure --version
pip install allure-pytest
pip install allure-python-commons
```
#### 3 import `import allure`
#### 4 Запуск
``` py
pytest --alluredir name_dir_allure_result name_test # запускаем тест с параметром --alluredir
# в терминале переходим в папку с созданной выше директорией allure => вводим
allure serve name_dir_allure_result
```
```py
# реализация скриншота с указанием шага
with allure.step('Open login page'):  
    allure.attach(browser.get_screenshot_as_png(), name='login_page_screen')
```

####  USING
[инструкция по настройке allure](https://allomart.ru/instruktsiya-po-nastroyke-allure/) - Сборка html-отчета на [[Jenkins]]
_____________
#### Links
[[Testing]]
[Official page](http://allure.qatools.ru/)
[pytest + Allure + Docker  Gitlab Pages + selenium](https://temofeev.ru/info/articles/testy-na-pytest-s-generatsiey-otchetov-v-allure-s-ispolzovaniem-docker-i-gitlab-pages-i-chastichno-s/)