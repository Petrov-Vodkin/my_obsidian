2021-11-17 12:53
[docs ru](https://docs-python.ru/standart-library/modul-subprocess-python/funktsija-run-modulja-subprocess/)
# subprocess
### Функция run() модуля subprocess в Python
 -запустить команду/программу с аргументами из кода Python.
#### Синтаксис:
```py
import subprocess

subprocess.run(args, *, stdin=None, input=None, stdout=None, 
               stderr=None, capture_output=False, shell=False, 
               cwd=None, timeout=None, check=False, 
               encoding=None, errors=None, text=None, 
               env=None, universal_newlines=None)
```
#### Параметры:
Аргументы функции, за исключением обязательного `args` - являются не обязательны и следовательно задаются только ключевыми словами.

-   `args` - запускаемая программа с аргументами,
-   `stdin=None` - поток данных, отправляемых в процесс,
-   `input=None` - поток данных, отправляемых в процесс,
-   `stdout=None` - поток вывода программы,
-   `stderr=None` - поток ошибок программы,
-   `capture_output=False` - захват вывода `stdout` и `stderr`,
-   `shell=False` - используйте `True`, если программа и ее аргументы представлены как одна строка,
-   `cwd=None` - путь к рабочему каталогу запускаемой программы,
-   `timeout=None` - максимально-допустимое время выполнения запущенной программы,
-   `check=False` - вызывает исключение при завершении запущенной программы с ошибками,
-   `encoding=None` - кодировка, если `input` - [строка](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-str-tekstovye-stroki/ "Тип данных str в Python."),
-   `errors=None` - [обработчик ошибок кодировки](https://docs-python.ru/standart-library/modul-codecs-python/obrabotchiki-oshibok-kodirovki/ "Обработчики ошибок кодировки."),
-   `text=None` - текстовый режим для `stdin`, `stdout` и `stderr`,
-   `env=None` - переменные среды для нового процесса,
-   `universal_newlines=None` - тоже, что и параметр `text`.

[Ключевые аргументы](https://docs-python.ru/tutorial/opredelenie-funktsij-python/kljuchevye-argumenty-opredelenii-funktsii/ "Ключевые аргументы в определении функции Python") этой функции это лишь наиболее распространенные параметры, правила использования которых описанными в разделе "[Часто используемые аргументы](https://docs-python.ru/standart-library/modul-subprocess-python/chasto-ispolzuemye-parametry-modulja-subprocess/ "Часто используемые параметры модуля subprocess в Python.")".

#### Возвращаемое значение:

-   экземпляр [`CompletedProcess()`](https://docs-python.ru/standart-library/modul-subprocess-python/obekt-completedprocess-modulja-subprocess/ "Объект CompletedProcess модуля subprocess в Python.") с результатами работы.

#### Описание:

Функция [`run()`](https://docs-python.ru/standart-library/modul-subprocess-python/funktsija-run-modulja-subprocess/ "Функция run() модуля subprocess в Python.") модуля [`subprocess`](https://docs-python.ru/standart-library/modul-subprocess-python/ "Модуль subprocess в Python, запуск новых процессов.") запускает команду/скрипт/программу с аргументами, ждет завершения команды, а затем возвращает экземпляр [`CompletedProcess()`](https://docs-python.ru/standart-library/modul-subprocess-python/obekt-completedprocess-modulja-subprocess/ "Объект CompletedProcess модуля subprocess в Python.") с результатами работы.

Полная сигнатура [функции `subprocess.run()`](https://docs-python.ru/standart-library/modul-subprocess-python/funktsija-run-modulja-subprocess/ "Функция run() модуля subprocess в Python.") в значительной степени совпадает с сигнатурой конструктора [`subprocess.Popen()`](https://docs-python.ru/standart-library/modul-subprocess-python/klass-popen-modulja-subprocess/ "Класс Popen() модуля subprocess в Python."), a большинство аргументов этой функции передаются в интерфейс последней кроме параметров `timeout`, `input`, `check` и `capture_output`.

Если аргумент `capture_output` () имеет значение `True`, то вывод `stdout` и `stderr` будут захвачены. При использовании, внутренний объект `subprocess.Popen()` автоматически создается с `stdout=subprocess.PIPE` и `stderr=subprocess.PIPE`. Аргументы `stdout` и `stderr` не могут быть предоставлены одновременно с `capture_output`. Если необходимо захватить и объединить оба потока в один, то используйте `stdout=subprocess.PIPE` и `stderr=subprocess.STDOUT` вместо `capture_output=True`.

Аргумент `timeout` передается в [`subprocess.Popen.communicate()`](https://docs-python.ru/standart-library/modul-subprocess-python/obekt-popen-modulja-subprocess/ "Объект Popen модуля subprocess в Python."). Если тайм-аут истекает, то дочерний процесс, в котором запущена программа будет принудительно остановлен и ожидание завершения процесса прекращается. [Исключение `subprocess.TimeoutExpired`](https://docs-python.ru/standart-library/modul-subprocess-python/iskljuchenija-modulja-subprocess/ "Исключения модуля subprocess в Python.") будет повторно вызвано после завершения дочернего процесса.

Аргумент `input` передается в `subprocess.Popen.communicate()` и следовательно в стандартный поток дочернего процесса. Если он используется, то это должна быть [последовательность байтов](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-bytes-bajtovye-stroki/ "Тип данных bytes, байтовые строки") или [строка](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-str-tekstovye-stroki/ "Тип данных str в Python.") - если задана кодировка `encoding` и `errors` или аргумент `text` имеет значение `True`. При использовании аргумента `input`, внутренний объект `subprocess.Popen` автоматически создается с помощью значения аргумента `stdin=subprocess.PIPE` при этом сам аргумент `stdin` может не указываться.

Если аргумент `check=True` и процесс завершается с ненулевым кодом выхода, то будет вызвано [исключение `subprocess.CalledProcessError`](https://docs-python.ru/standart-library/modul-subprocess-python/iskljuchenija-modulja-subprocess/ "Исключения модуля subprocess в Python."). Атрибуты этого исключения содержат аргументы запуска программы, код ее выхода, а так же `stdout` и `stderr`, если они были захвачены.

Если указаны кодировка `encoding` или `errors` или аргумент `text` имеет значение `True`, то файловые объекты для [`stdin`, `stdout` и `stderr`](https://docs-python.ru/standart-library/modul-sys-python/obekty-stdin-stdout-stderr-modulja-sys/ "Объекты stdin, stdout, stderr модуля sys в Python.") открываются в текстовом режиме с использованием указанной кодировки и обработчика ошибок или значения по умолчанию [`io.TextIOWrapper()`](https://docs-python.ru/standart-library/modul-io-python/klass-io-textiowrapper-modulja-io/ "Класс io.TextIOWrapper модуля io в Python.").

Значение аргумента `universal_newlines` эквивалентно аргументу `text` и предназначен для обратной совместимости. По умолчанию [файловые объекты](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-file-object-fajly-potoki/ "Тип файлового объекта file object в Python.") открываются в [двоичном режиме](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-bytes-bajtovye-stroki/ "Тип данных bytes, байтовые строки").

Если аргумент `env` не равен `None`, то это должен быть [словарь](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-dict-slovar/ "Тип данных dict, словарь"), который определяет [переменные среды](https://docs-python.ru/standart-library/modul-os-python/upravlenie-sredoj-okruzhenija-koda/ "Управление переменной средой окружения системы в Python.") для нового процесса. Он используются вместо поведения по умолчанию - наследования среды текущего процесса. Аргумент `env` непосредственно передается в конструктор [`subprocess.Popen()`](https://docs-python.ru/standart-library/modul-subprocess-python/klass-popen-modulja-subprocess/ "Класс Popen() модуля subprocess в Python.").

### Примеры исполнения команд/программ из кода в Python.
```py
import subprocess
result = subprocess.run(['ls', '-ls', '/tmp'])
# итого 44
# 4 drwxrwxr-x 2 docs-python docs-python 4096 мая 12 09:39  closeditems
# 0 srwx------ 1 sddm   sddm      0 мая 12 08:51  sddm-:0-gYOiIB
# 4 -rw------- 1 docs-python docs-python  110 мая 12 09:03  xauth-1000-_0
# ...
```
Чтобы вызывать команды, в которых используются wildcard-выражения `*tpl`, нужно добавлять аргумент `shell`.
```py
import subprocess
result = subprocess.run('ls -ls /tmp/*tpl', shell=True)
# 4 -rw------- 1 docs-python docs-python 317 мая 12 09:56 /tmp/tmp-26999MFPH5MgIW157.tpl
result.returncode # 0
```
Получение результата выполнения команды, если не указать параметр `encoding`, то результат получим как [байтовую строку](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-bytes-bajtovye-stroki/ "Тип данных bytes, байтовые строки").
```py
import subprocess
result = subprocess.run('ls -ls /tmp/*tpl', shell=True, \
                            stdout=subprocess.PIPE, encoding='utf-8')
result.stdout		# '4 -rw------- 1 docs-python docs-python 317 мая 12 09:56 /tmp/tmp-26999MFPH5MgIW157.tpl\n'
result.returncode	# 0
```
Отключение вывода.
```py
import subprocess
result = subprocess.run(['ls', '-ls', '/tmp/'], stdout=subprocess.DEVNULL)
print(result.stdout)# None
result.returncode	# 0
result.args			# ['ls', '-ls', '/tmp/']
```
Если команда была выполнена с ошибкой или не отработала корректно, вывод команды попадет на стандартный поток ошибок.
Получить этот вывод можно так же, как и стандартный поток вывода:
```py
import subprocess
result = subprocess.run(['ping', '-c', '3', '-n', 'host.host'], \
                            stderr=subprocess.PIPE, encoding='utf-8')
result.stderr	# 'ping: host.host: Неизвестное имя или служба\n'
```
_____________
#### Links
