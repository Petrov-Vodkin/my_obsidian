2020-12-11 12:52
#плагины #параметры #pip #package #модули
# Плагины PyTest
#### [[Паралельный запуск тестов pytest]]
```bash
pip install pytest-xdist
# -n4 паралельный запуск на 8-ми процессах, без '-n'- обычный запуск
pytest -n4 -s -v --tb=line --reruns 2 -m need_review 
```
#### pytest-rerunfailures (перезапуск тестов)
```bash
pip install pytest-rerunfailures
--reruns n 		#  n — это количество перезапусков
```
#### [Параметры вывода данных о ходе тестов](https://docs.pytest.org/en/latest/usage.html#modifying-python-traceback-printing)
```bash
pytest -v --tb=line --reruns 1 --browser_name=chrome`
--tb=line # сократить лог с результатами теста(fail => одна строка)
```
#### pytest-ordering 
```python
# позволяет задавать вручную порядок запуска тестов через метку Run.
import pytest

@pytest.mark.run(order=2)
def test_foo():
    assert True

@pytest.mark.run(order=1)
def test_bar():
    assert True
```
#### pytest-smartcov `позволяет проверять покрытие кода тестам, как полное, так и частичное.`
#### pytest-timeout `позволяет завершать тесты по таймауту, через параметр командной строки или специальной метки.`
#### pytest-bdd ???
`pytest` может использоваться для запуска тестов, которые выходят за рамки традиционной области модульного тестирования. [Разработка на основе поведения](https://en.wikipedia.org/wiki/Behavior-driven_development) (BDD) поощряет написание на понятном языке описания вероятных действий и ожиданий пользователя, которые затем можно использовать для определения необходимости реализации данной функции. [pytest-bdd](https://pytest-bdd.readthedocs.io/en/latest/) помогает вам использовать [Gherkin](http://docs.behat.org/en/v2.5/guides/1.gherkin.html) для написания функциональных тестов для вашего кода.
-   [PyTest-localserver](https://bitbucket.org/basti/pytest-localserver/) незаменим, когда нужно эмулировать какой либо из сервисов.
-   [PyTest-timeout](https://pypi.python.org/pypi/pytest-timeout) используем для тесткейсов, которые должны проверять таймауты сервисов.
-   [PyTest-xdist](https://pypi.python.org/pypi/pytest-xdist) — musthave, если вы хотите распараллелить запуск тестов.
-   [PyTest-capturelog](https://pypi.python.org/pypi/pytest-capturelog) удобен при тестировании кода, который использует модуль `logging`.
-   _PyTester_ используем постоянно, когда нужно проверить матчеры или написать тест на фикстуру и др. Уже входит в состав **pytest**.
-   [PyTest-httpretty](https://pypi.python.org/pypi/pytest-httpretty) перехватывает запросы на определенный `uri`, позволяя подставляет заранее заданный ответ. Вместо него используем `pytest-localserver`
-   [PyTest-ordering](https://pypi.python.org/pypi/pytest-ordering) — отказались от использования. Как-то потребовалось сконфигурировать запуск тестов в определенной последовательности.
-   [PyTest-incremental](https://pypi.python.org/pypi/pytest-incremental) удобно, если вы используете статичный сервер, на котором крутятся тесты, и хотите, чтобы тесты запускались после изменений в коде. Мы перешли на Jenkins.
#### Links
[[PyTest]] [[Python]] 