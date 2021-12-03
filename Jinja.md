2021-09-11 13:49
[en doc](https://jinja.palletsprojects.com/en/2.11.x/) | [ru doc](https://docs-python.ru/packages/modul-jinja2-python/) `python -m pip install -U Jinja2`
# Jinja
### Общий синтаксис шаблона jinja2 в Python[](https://docs-python.ru/packages/modul-jinja2-python/sintaksis-shablona-jinja2/)
```py
{{ }}	# получить результат выражение, переменную или вызвать функцию и вывести значение в шаблоне

```
[](https://pythonru.com/uroki/7-osnovy-shablonizatora-jinja)
### Инициализация движка шаблонов Jinja2 в Python.
Модуль `Jinja` использует центральный объект, называемый шаблоном [`jinja2.Environment()`](https://docs-python.ru/packages/modul-jinja2-python/klass-environment/ "Класс Environment() модуля jinja2 в Python."). Экземпляры этого класса используются для хранения конфигурации и глобальных объектов, а также для [загрузки шаблонов](https://docs-python.ru/packages/modul-jinja2-python/zagruzchiki-shablonov-modulja-jinja2/ "Загрузчики шаблонов модуля jinja2 в Python.") из файловой системы или других мест. Даже если создавать шаблоны из строк с помощью конструктора класса [`jinja2.Template()`](https://docs-python.ru/packages/modul-jinja2-python/klass-template/ "Класс Template() модуля jinja2 в Python."), среда `Environment` создается автоматически, только она будет совместно используемая.
Самый простой способ настроить Jinja для загрузки шаблонов для приложения выглядит примерно так:
```py
from jinja2 import Environment, PackageLoader, select_autoescape
env = Environment(
	# создаст шаблонную среду с настройками по умолчанию и загрузчиком `loader`,
    loader=PackageLoader('yourapplication', 'templates'), #  будет искать шаблоны в папке шаблонов `templates` внутри пакета python `yourapplication`
	# автоматическое экранирование файлов с расширениями `.html` и `.xml`.
    autoescape=select_autoescape(['html', 'xml'])
)
```
[Доступны разные загрузчики](https://docs-python.ru/packages/modul-jinja2-python/zagruzchiki-shablonov-modulja-jinja2/ "Загрузчики шаблонов модуля jinja2 в Python."). 

Чтобы загрузить шаблон из среды `env`, которая определены в примере выше, нужно просто вызвать метод `env.get_template()`, который затем возвращает загруженный шаблон [`Template`](https://docs-python.ru/packages/modul-jinja2-python/klass-template/ "Класс Template() модуля jinja2 в Python."):
```py
template = env.get_template('mytemplate.html')
#Template.render(): отобразить шаблон с некоторыми переменными
print(template.render(the='variables', go='here')) 
```
Использование [загрузчика шаблонов](https://docs-python.ru/packages/modul-jinja2-python/zagruzchiki-shablonov-modulja-jinja2/ "Загрузчики шаблонов модуля jinja2 в Python.") вместо передачи обычных [строк](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-str-tekstovye-stroki/ "Тип данных str в Python.") в `Template` или `Environment.from_string()` имеет несколько преимуществ. Помимо того, что загрузчик намного проще в использовании, он также позволяет [наследование шаблонов](https://docs-python.ru/packages/modul-jinja2-python/nasledovanie-shablonov-jinja2/ "Наследование шаблонов jinja2 в Python.").
### Примечания по автоэкранированию.
В будущих версиях Jinja, автоматическое экранирование будет включено по умолчанию из соображений безопасности. Рекомендуется явно [настроить автоматическое экранирование](https://docs-python.ru/packages/modul-jinja2-python/funktsija-select-autoescape/ "Настройка экранирования в шаблонах Jinja в Python."), а не полагаться на значение по умолчанию.
#### Примечания по идентификаторам в шаблонах.
Jinja использует правила именования Python. Допустимые идентификаторы могут быть любой комбинацией символов Юникода, принятой Python.
[Фильтры](https://docs-python.ru/packages/modul-jinja2-python/vstroennye-filtry-modulja-jinja2/ "Встроенные фильтры модуля jinja2 в Python.") ищутся в отдельных пространствах имен и имеют слегка измененный синтаксис идентификатора. Фильтры могут содержать точки для группировки по темам. Например, вполне допустимо добавить метод в фильтр и вызвать его как `.unicode`. Регулярное выражение для проверки идентификаторов фильтров: `[a-zA-Z_][a-zA-Z0-9_]*(\.[a-zA-Z_][a-zA-Z0-9_]*)*`.

## Базовое использование движка шаблонов Jinja2.

В этом разделе дается краткое введение в Python API для шаблонов Jinja.

Самый простой способ создать шаблон и отрендерить его - использовать класс [`jinja2.Template()`](https://docs-python.ru/packages/modul-jinja2-python/klass-template/ "Класс Template() модуля jinja2 в Python."). Такой способ работы не рекомендуется, если шаблоны загружаются не из строк, а из файловой системы или другого источника данных:
```py
 from jinja2 import Template
 template = Template('Hello {{ name }}!')
 template.render(name='John Doe')
# 'Hello John Doe!'

 content = {'a': 5, 'b': 2}
 tpl = 'Сумма чисел {{ a }} и {{ b }} равна {{ a + b }}'
 Template(tpl).render(content)
# 'Сумма чисел 5 и 2 равна 7'
```
### Пример разбора шаблона с циклом.

```py
 import jinja2
# шаблон с циклом
 tpl = """{{ title }}
... {{ '-' * title|length }}
... {% for n, user in enumerate(users, 1) %}
... {{ n }}. {{ user.name }} - должность: {{ user.status }}, оклад: ${{ user.salary }}
... {% endfor %}
... """
# собираем данные для шаблона
 content = {}
 content['title'] = 'Итерация по пользователям'
 content['users'] = []
 content['users'].append({'name': 'Маша', 'status': 'Менеджер', 'salary': 1500}) 
 content['users'].append({'name': 'Света', 'status': 'Дизайнер', 'salary': 1000}) 
 content['users'].append({'name': 'Игорь', 'status': 'Программист', 'salary': 2000}) 
# В словаре передаем в шаблон функцию Python
 content['enumerate'] = enumerate
# Смотрим, что получилось
 print(jinja2.Template(tpl, trim_blocks=True).render(content))
# Итерация по пользователям
# -------------------------
# 1. Маша - должность: Менеджер, оклад: $1500
# 2. Света - должность: Дизайнер, оклад: $1000
# 3. Игорь - должность: Программист, оклад: $2000
```
### Загрузка шаблонов из файловой системы.
Сохраним шаблон из предыдущего примера в директорию `~/temp/main.txt`.
```py
{# файл ~/temp/main.txt #}
{{ title }}
{# Добавим условие #}
{% if title %}
{# Если существует переменная `title`, то будем ее подчеркивать #}
{{ '-' * title|length }}
{% endif %}
{# Цикл по пользователям #}
{% for n, user in enumerate(users, 1) %}
{{ n }}. {{ user.name }} - должность: {{ user.status }}, оклад: ${{ user.salary }}
{% endfor %}
```
Основной код программы, которая работает с сохраненным шаблоном `main.txt`. Для понимания, что происходит, код снабжен подробными комментариями. [Класс Environment() модуля jinja2.](https://docs-python.ru/packages/modul-jinja2-python/klass-environment/ "Среда окружения движка шаблонов jinja2.")
```py
import jinja2
 
# Определяем класс загрузчика шаблонов из файловой системы
# (`temp` - папка где лежит сохраненный шаблон 'main.txt')
loader = jinja2.FileSystemLoader('temp')
# Определяем переменную среду, 
# в которую передаем загрузчик
env = jinja2.Environment(loader=loader, trim_blocks=True)

# данные для шаблона
content = {}
content['title'] = 'Итерация по пользователям'
content['users'] = []
content['users'].append({'name': 'Маша', 'status': 'Менеджер', 'salary': 1500}) 
content['users'].append({'name': 'Света', 'status': 'Дизайнер', 'salary': 1000}) 
content['users'].append({'name': 'Игорь', 'status': 'Программист', 'salary': 2000}) 
# В словаре передаем в шаблон функцию Python
content['enumerate'] = enumerate

# загружаем шаблон 'main.txt'
tpl = env.get_template('main.txt')
# рендерим шаблон в переменную `result`
result = tpl.render(content)
# Сохраним получившийся текст
with open('result.txt', 'w') as fp: 
    fp.write(result)

# Прочитаем записанный файл
with open('result.txt', 'r') as fp: 
    print(fp.read())

# Итерация по пользователям
# -------------------------
# 1. Маша - должность: Менеджер, оклад: $1500
# 2. Света - должность: Дизайнер, оклад: $1000
# 3. Игорь - должность: Программист, оклад: $2000
```
_____________
#### Links
[DOCS-Python.ru](https://docs-python.ru/ "Справочная документация по языку Python3.")™
https://pythonru.com/uroki/7-osnovy-shablonizatora-jinja
['Кепки': SimpleServer + Jinja](https://dvmn.org/encyclopedia/modules/jinja2/)
[youtube self_du](https://www.youtube.com/playlist?list=PLA0M1Bcd0w8wfmtElObQrBbZjY6XeA06U)