2020-12-03 16:35
#pytest | [документация на русском](https://pytest-docs-ru.readthedocs.io/ru/latest/contents.html) | [documentation en](https://docs.pytest.org/en/latest/contents.html) 
# PyTest
-фреймворк для упащения написания и использования автотестов
#### Возможности `pytest`[](https://pytest-docs-ru.readthedocs.io/ru/latest/index.html#id2 "Ссылка на этот заголовок")
-   подробный разбор упавших проверок [assert](https://pytest-docs-ru.readthedocs.io/ru/latest/assert.html#id1) (не нужно помнить имена `self.assert*`);
-   [автообнаружение](https://pytest-docs-ru.readthedocs.io/ru/latest/goodpractices.html#test-discovery) тестовых модулей и функций;
-   использование [модульных фикстур](https://pytest-docs-ru.readthedocs.io/ru/latest/fixture.html#fixture) для управления небольшими или параметризованными тестовыми ресурсами;
-   запуск тестовых наборов, написанных с использованием [unittest](https://pytest-docs-ru.readthedocs.io/ru/latest/unittest.html#unittest) (включая пробные) и [nose](https://pytest-docs-ru.readthedocs.io/ru/latest/nose.html#noseintegration);
-   у `pytest` есть большой набор (315 + [внешних плагинов](http://plugincompat.herokuapp.com)) и процветающее сообщество.

#### Запуск, фикстуры, параметризация, метки, 
* [[Integrated pytest in  Pycharm]]
* запуск [[Запуск pyTest]] | [[Паралельный запуск тестов pytest]] + [[Allure]]
* [[Маркировка тестов]] + [stepik](https://stepik.org/lesson/236918/step/2) 
* [[Fixtures]]
* [[async test patterns for Pytest]]

#### Изменение вывода сообщений трассировки[](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#id5 "Ссылка на этот заголовок")
Примеры вывода:
```shell
pytest --showlocals # показывать локальные переменные в сообщениях
pytest -l           # показывать локальные переменные в сообщениях (краткий вариант)
pytest --tb=auto    # (по умолчанию) "расширенный" вывод для первого и
                    # последнего сообщений, и "короткий" для остальных
pytest --tb=long    # исчерпывающий, подробный формат сообщений
pytest --full-trace	# при ошибке печатаются очень длинные трассировки (длиннее, чем при `--tb=long`).  гарантирует, что сообщения трассировки будут напечатаны при **прерывании выполнения c клавиатуры** с помощью Ctrl+C
pytest --tb=short   # сокращенный формат сообщений
pytest --tb=line    # только одна строка на падение
pytest --tb=native  # стандартный формат библиотеки Python
pytest --tb=no      # никаких сообщений
```

#### [Параметры](https://docs.pytest.org/en/latest/usage.html#modifying-python-traceback-printing) запуска
```bash
# лучше проще запусти venv (выход из venv: deactivate)
pytest -r				# отображения «краткой сводной информации по тестированию»
pytest -ra
	или -rA test_NAME.py# для вывода всех дополнительных сообщений 
pytest -rx
	или -rX -v test_1.py# покажет на прохождение теста с маркировкой xfail [(ожидаемо падающий)](https://stepik.org/lesson/236918/step/5) https://docs.pytest.org/en/latest/skipping.html#xfail-mark-test-functions-as-expected-to-fail
# Параметр -r принимает ряд символов после себя. Использованный выше символ а означает “все, кроме успешных».
```
[Вот полный список доступных символов, которые можно использовать:](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#pytest-python-m-pytest)
```bash
			f 	# упавшие (добавляет раздел FAILED)
			E 	# ошибки (добавляет раздел ERROR)
			s 	# пропущенные (добавляет раздел SKIPPED)
			x 	# тесты XFAIL (добавляет раздел XFAIL)
			X 	# тесты XPASS (добавляет раздел XPASS)
			p 	# успешные (passed)
			P 	# успешные (passed) с выводом
			# Есть и специальные символы для пропуска отдельных групп:
			a # выводить все, кроме pP
			A # выводить все
			N # ничего не выводить (может быть полезным, поскольку по умолчанию используется комбинация fE).
pytest -rfs				# увидеть только упавшие и пропущенные тесты			
pytest --runxfail  		# игнорирование xfail
pytest -s test_NAME.py	# (увидеть текст, который выводится командой "print()")
pytest -m test_NAME.py	# (запуск теста с маркировкой)[[Маркировка тестов]]
```
### Вызов `pytest` из кода Python[](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#pytest-python "Ссылка на этот заголовок")
```py
pytest.main()	# вызвать pytest прямо в коде Python
				# передавать параметры и опции:
pytest.main(["-x", "mytestdir"])
```
Такой способ эквивалентен вызову «pytest» из командной строки. В этом случае вместо исключения `SystemExit` возвращается статус завершения.

### Запуск отладчика [PDB](http://docs.python.org/library/pdb.html) (Python Debugger) при падении тестов[](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#pdb-python-debugger "Ссылка на этот заголовок")

`python` содержит встроенный отладчик [PDB](http://docs.python.org/library/pdb.html) (Python Debugger). `pytest` позволяет запустить отладчик с помощью параметра командной строки:
```shell
pytest --pdb	# запускать отладчик при каждом падении теста (или прерывании его с клавиатуры). чтобы понять причину его падения:

pytest -x --pdb   # вызывает отладчик при первом падении и завершает тестовую сессию
pytest --pdb --maxfail=3  # вызывает отладчик для первых трех падений
```
Обратите внимание, что при любом падении информация об исключении сохраняется в `sys.last_value`, `sys.last_type` и `sys.last_traceback`. При интерактивном использовании это позволяет перейти к отладке после падения с помощью любого инструмента отладки. Можно также вручную получить доступ к информации об исключениях, например:
```py
>>> import sys
>>> sys.last_traceback.tb_lineno
42
>>> sys.last_value
AssertionError('assert result == "ok"',)
```
### Запуск отладчика [PDB](http://docs.python.org/library/pdb.html) (Python Debugger) в начале теста[](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#trace-option "Ссылка на этот заголовок")
`pytest` позволяет запустить отладчик сразу же при старте каждого теста. Для этого нужно передать следующий параметр:
```shell
pytest --trace	# отладчик будет вызываться при запуске каждого теста.
```
### Установка точек останова[](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#breakpoints "Ссылка на этот заголовок")
Чтобы установить точку останова, вызовите в коде `import pdb;pdb.set_trace()`, и `pytest` автоматически отключит перехват вывода для этого теста, при этом:
-   на перехват вывода в других тестах это не повлияет;
-   весь перехваченный ранее вывод будет обработан как есть;
-   перехват вывода возобновится после завершения отладочной сессии (с помощью команды `continue`).

### Использование встроенной функции breakpoint[](https://pytest-docs-ru.readthedocs.io/ru/latest/usage.html#breakpoint "Ссылка на этот заголовок")

`Python 3.7` содержит встроенную функцию `breakpoint()`. `pytest` поддерживает использование `breakpoint()` следующим образом:
-   если вызывается `breakpoint()`, и при этом переменная `PYTHONBREAKPOINT` установлена в значение по умолчанию, `pytest` использует расширяемый отладчик [PDB](http://docs.python.org/library/pdb.html) вместо системного;
-   когда тестирование будет завершено, система снова будет использовать отладчик `Pdb` по умолчанию;
-   если `pytest` вызывается с опцией `--pdb` то расширяемый отладчик [PDB](http://docs.python.org/library/pdb.html) используется как для функции `breakpoint()`, так и для упавших тестов/необработанных исключений;
-   для определения пользовательского класса отладчика можно использовать `--pdbcls`.

### [[Плагины PyTest]]
* pytest-rerunfailures - перезапуск упавшего теста 

#### Links
[[Python]] 