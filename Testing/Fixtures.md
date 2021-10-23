2020-12-03 18:35
#fixtures #фикстуры #декораторы #conftest #декоратор
# Fixtures
Фикстуры в контексте PyTest — это вспомогательные функции декораторы для наших тестов, которые не являются частью тестового сценария.
[[Маркировка тестов]]
#### ***Перредача параметров в декоратор(фикстуру)*** через командную строку:
Это делается с помощью встроенной функции ***pytest_addoption*** и фикстуры ***request***. Сначала добавляем в файле conftest обработчик опции в функции pytest_addoption, затем напишем фикстуру, которая будет обрабатывать переданные в опции данные. [Подробнее](https://docs.pytest.org/en/latest/example/simple.html?highlight=addoption)

[conftest.py:](https://stepik.org/lesson/237240/step/6?unit=209628)
```py
import pytest
from selenium import webdriver

def pytest_addoption(parser):
	parser.addoption('--browser_name', action='store', default=None,
					 help="Choose browser: chrome or firefox")


@pytest.fixture(scope="function")
def browser(request):
	browser_name = request.config.getoption("browser_name")
	browser = None
	if browser_name == "chrome":
		print("\nstart chrome browser for test..")
		browser = webdriver.Chrome()
	elif browser_name == "firefox":
		print("\nstart firefox browser for test..")
		browser = webdriver.Firefox()
	else:
		raise pytest.UsageError("--browser_name should be chrome or firefox")
	yield browser
	print("\nquit browser..")
	browser.quit()

```

Классический способ работы с фикстурами — создание setup- и teardown-методов в файле с тестами ([документация в PyTest](https://docs.pytest.org/en/latest/xunit_setup.html#module-level-setup-teardown)).

setup_class(self) и teardown_class(self) вместе с ***@classmethod*** запускаются один раз перед всеми методами (setup_class(self)) и закрываются также один раз (teardown_class(self)). https://stepik.org/lesson/237257/step/2?unit=209645

А в случае с setup_method(self) и teardown_method(self) оборачивается каждый исполняемый метод(функция def name_func(self))

**PyTest** предлагает продвинутый подход к **фикстурам**, когда фикстуры можно задавать глобально, передавать их в тестовые методы как параметры, а также имеет набор встроенных фикстур. 
**ФИНАЛИЗАТОР** — закрываем браузер
Один из вариантов финализатора — использование ключевого слова Python: yield. После завершения теста, который вызывал фикстуру, выполнение фикстуры продолжится со строки, следующей за строкой со словом yield:
```py
import pytest
from selenium import webdriver
link = "http://selenium1py.pythonanywhere.com/"

@pytest.fixture
def browser():
	print("\nstart browser for test..")
	browser = webdriver.Chrome()
	yield browser
	# этот код выполнится после завершения теста(при повторном вызове browser)
	print("\nquit browser..")
	browser.quit()
class TestMainPage1():
	# вызываем фикстуру в тесте, передав ее как параметр
	def test_guest_should_see_login_link(self, browser):
		browser.get(link)
		browser.find_element_by_css_selector("#login_link")
def test_guest_should_see_basket_link_on_the_main_page(self, browser):
	browser.get(link)
	browser.find_element_by_css_selector(".basket-mini .btn-group > a")
```
### ***ВОЗВРАЩАЕМОЕ ЗНАЧЕНИЕ***
Фикстуры могут возвращать значение, которое затем можно использовать в тестах.  Мы создадим фикстуру browser, которая будет создавать объект WebDriver. Мы напишем метод browser и укажем, что он является фикстурой с помощью декоратора ****@pytest.fixture****. После этого мы можем вызывать фикстуру в тестах, передав ее как параметр. По умолчанию фикстура будет создаваться для каждого тестового метода, то есть для каждого теста запустится свой экземпляр браузера.
import pytest
```py
from selenium import webdriver
link = "http://selenium1py.pythonanywhere.com/"

@pytest.fixture		#создаём фикстуру
def browser():
	print("\nstart browser for test..")
	browser = webdriver.Chrome()
	return browser

class TestMainPage1():
	# вызываем фикстуру в тесте, передав ее как параметр(browser)
	def test_guest_should_see_login_link(self, browser): 
		browser.get(link)
		browser.find_element_by_css_selector("#login_link")

	def test_guest_should_see_basket_link_on_the_main_page(self, browser):
		browser.get(link)
		browser.find_element_by_css_selector(".basket-mini .btn-group > a")
```
### **ОБЛАСТИ ВИДИМОСТИ scope**
Для фикстур можно задавать область покрытия фикстур. Допустимые значения: “function”, “class”, “module”, “session”.
	
	@pytest.fixture(scope=“function” or “class” or “module” or “session”)
Соответственно, фикстура будет вызываться один раз для тестового метода, один раз для класса, один раз для модуля или один раз для всех тестов, запущенных в данной сессии. 
Дополнительный параметр **autouse=True**, который укажет, что фикстуру нужно запустить для каждого теста даже без явного вызова: 

	@pytest.fixture(autouse=True)
### [Conftest.py — конфигурация тестов](https://docs.pytest.org/en/latest/fixture.html?highlight=conftest#override-a-fixture-on-a-folder-conftest-level)
Для хранения часто употребимых фикстур и хранения глобальных настроек нужно использовать файл conftest.py
* создаём фаил conftest.py в дерриктории ***КАЖДОГО*** проэкта
	
		selenium_course_solutions/
		├── section3
		│   └── conftest.py
		│   └── test_languages.py
		├── section4 
		│   └── conftest.py
		│   └── test_main_page.py
* заполняем его фикстурами
conftest.py:  
```py
import pytest
from selenium import webdriver

@pytest.fixture(scope="function")
def browser():
	print("\nstart browser for test..")
	browser = webdriver.Chrome()
	yield browser
	print("\nquit browser..")
	browser.quit()
```
* для использования - фикстура передается в тестовый метод в качестве аргумента
test_conftest.py:
```py
link = "http://selenium1py.pythonanywhere.com/"

def test_guest_should_see_login_link(browser): #пердача фиксуры 
	browser.get(link)
	browser.find_element_by_css_selector("#login_link")
```
#### Links
[[Python]] | [[PyTest]] | [[Decorators]]
