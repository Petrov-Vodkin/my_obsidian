## Запуск 
#xfail #skip
##### [[Headless Chrome]] запуск без создания визуального окна браузер
```bash
pytest -v --tb=line -m name_marker
# --tb=line выводить только одну строку из лога каждого упавшего теста
```
-если мы не передали никакого аргумента в команду, а написали просто `pytest`, тест-раннер начнёт поиск в текущей директории
 -как аргумент можно передать файл, путь к директории или любую комбинацию директорий и файлов, например: 
```bash
pytest scripts/selenium_scripts  "-найти все тесты в директории scripts/selenium_scripts"
pytest test_user_interface.py "-найти и выполнить все тесты в файле "
pytest scripts/drafts.py::test_register_new_user_parametrized "-найти тест с именем
test_register_new_user_parametrized в указанном файле в указанной директории и выполнить "
```
#### Поиск тесвов pytest-ом
```
# Cвоего рода псевдокод 
if name_of_file == (test_*.py OR *_test.py)
	if def_name_outOfClass == "test" + def_name
			if class_name == "Test" + className
			if def_name_InClass == "test" + def_name
```

### Запуск тестов с маркировкой
#### ***Все кроме ....***
можно использовать инверсию. Для запуска всех тестов, не отмеченных как smoke, нужно выполнить команду:

```Bash
pytest -s -v -m "not smoke" test_fixture8.py
```
#### ***Разными метки ***
можно использовать логическое ИЛИ. Запустим smoke и regression-тесты:
```Bash
pytest -s -v -m "smoke or regression" test_fixture8.py
```
#### Несколько маркировок	
```Bash
pytest -s -v -m "smoke and win10" test_fixture81.py
```
### [ Пропустить тест](https://pytest-docs-ru.readthedocs.io/ru/latest/skipping.html)
Итак, чтобы пропустить тест, его отмечают в коде как @pytest.mark.skip:	
```Python
	@pytest.mark.skip
    def test_guest_should_see_login_link(self, browser):
        browser.get(link)
        browser.find_element_by_css_selector("#login_link")
```		
#### @pytest.mark.xfail для падающего теста
```Python	    
@pytest.mark.xfail
def test_guest_should_see_search_button_on_the_main_page(self, browser):
	browser.get(link)
	browser.find_element_by_css_selector("button.favorite")
```

### Tag
[[Python]],[[PyTest]], [[Маркировка тестов]],[[Xfail_Skip]]
