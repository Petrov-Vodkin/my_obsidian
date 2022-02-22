2021-09-11 13:49
[en doc](https://jinja.palletsprojects.com/en/2.11.x/) | [ru doc](https://docs-python.ru/packages/modul-jinja2-python/) [&](http://xgu.ru/wiki/Jinja2) `python -m pip install -U Jinja2`
# Jinja
### 2. Шаблонизатор
[Русские Блоги](https://russianblogs.com/article/55171430056/)
В Flask использование механизма шаблонов в основном разделено на следующие три этапа:
```python
#  Сначала объявите данные
uname = 'WHOAMI'`
# Во-вторых, напишите шаблон
tpl = '<h1>Welcome,{{username}}</h1>'
# В-третьих, отправьте рендеринг механизма шаблонов
render_template_string(tpl,username=uname)
```
Код эксперимента следующий:
```python
# -*- coding:utf-8 -*-
from flask import Flask,render_template_string
app = Flask(__name__)
@app.route('/')def v_index():
	uname = 'WHOAMI'    
	tpl = '<h1>Welcome,{{username}}</h1>'    
	return render_template_string(tpl,username=uname) 
app.run(host='0.0.0.0',port=8080)
```

Страница эксперимента выглядит следующим образом:

![10012631_H0Ib.png](https://russianblogs.com/images/525/39745ee759031a0048e70642a305f94d.png)  

### 3. Визуализация шаблона

Flask основан на движке шаблонов Jinja2 и предоставляет две функции рендеринга, которые используют строки или отдельные файлы для сохранения содержимого шаблона:

-   render_template_string (sourcestr, ** context) - использовать sourcestr как строку шаблона
    
-   render_template (имя файла, ** контекст) - использовать содержимое файла, указанного в имени файла, в качестве строки шаблона
    

**render_template_string** : В следующем примере используется довольно простой шаблон для отправки персонализированных приветственных сообщений разным пользователям:

```python
@app.route('/user/<uname>')
def show_user_profile(uname):
	return render_template_string('<h1>Welcome,{{ uname }}</h1>',uname=uname)
```

В грамматике Jinja2,_{{varibale}}_Представляет команду вывода. Всякий раз, когда механизм рендеринга находит команду вывода, она находится в результате рендеринга с использованием переменных в контексте данных шаблона._variable_Замените исходную команду вывода значением.

**render_template** : В производственной среде не рекомендуется писать строки шаблона в коде. Серьезный способ - написать шаблон в отдельном файле шаблона и использовать функцию render_template () для рендеринга:

```python
@app.route('/user/<username>')def v_user(username):  
	return render_template('user.html',username=username)
```
По умолчанию Flask использует_templates_Подкаталог используется как каталог шаблона, а файл user.html должен быть помещен в эту папку:
```python
/app /web.py /templates   /user.html
```
Содержимое user.html выглядит следующим образом:
```python
<body>   <h1>Welcome, {{ username }}</h1></body>
```

### 4. Переменные и выражения

Переменные шаблона передаются при рендеринге шаблона_Контекстный словарь_Переходим к шаблону. В приведенном ниже примере в шаблоне используются переменные._name_с участием_age_При звонке_render_template_string()_При рендеринге шаблона передайте значения двух переменных через аргументы ключевого слова:

```python
tpl = 'name : {{ name }} age : {{age}} 'print render_template_string(tpl,name='Marion5',age=12)
```

**Переменный член** : Если переменная, переданная в шаблон, является не простым типом Python, а типом словаря или объекта, то к атрибутам или методам элементов можно получить доступ в шаблоне так же, как и в Python.

Небольшое отличие состоит в том, что для словарных переменных, помимо_[]_Способ доступа к его участникам, вы также можете использовать_._：

```python
tpl = 'name: {{u["name"]}} name again:{{u.name}}'print render_template_string(tpl,u={'name':'Marion5','age':12})
```

Аналогично для объектных переменных, помимо использования_._Чтобы получить доступ к значению атрибута, вы также можете использовать_[]_：

```python
class User: def __init__(self,name,age):   self.name = name   self.age = age tpl = 'name : {{u.name}} name again:{{u["name"]}}'print render_template_string(tpl,u=User('Mary',20))
```

**выражение** : Переменные также могут применяться к выражениям, таким как математические операции, те обычно используемые математические операторы (+ - _/ // % *_ ) Все действительны:

```python
data = {'x':12,'y':13}tpl = '{{x}} + {{y}} = {{ x+y }}'print render_template_string(tpl,**data)
```

Или сравните или логические операции (==! =>> = << = и или нет):

```python
data = {'x':12,'y':13,'z':11}tpl = '{{x}} > {{y}} : {{ x>y }}'print render_template_string(tpl,**data)
```

**Вызов функции** : В выходной команде вы можете выполнять вызовы функций для переменных или констант:

```python
tpl = '{{ range(10) }} 'print render_template_string(tpl)
```

Важно отметить, что у шаблона есть собственный_Глобальный домен / глобалы_, Поэтому здесь_range()_Функции не являются функциями в приложениях Python.

### 5. Глобальные объекты

Встроенные глобальные объекты Jinja2 включают:

-   range([start, ]stop[, step])
    
-   lipsum(n=5, html=True, min=20, max=100)
    
-   dict(**items)
    
-   class cycler(*items)
    
-   class joiner(sep=', ')
    

Flask внедряет следующие глобальные объекты в шаблон Jinja2, к которым можно получить доступ прямо в шаблоне:

-   config-объект конфигурации текущего экземпляра приложения Flask
    
-   request-текущий объект HTTP-запроса
    
-   session-объект сеанса текущего HTTP-запроса
    
-   g-глобальный объект в текущем цикле HTTP-запроса
    
-   url_for () - функция генерации URL
    
-   get_flashed_messages () - функция флэш-сообщения
    

В следующем примере текущее имя пользователя извлекается из сеанса:

```python
@app.route('/')def v_index(): tpl = 'welcome back, {{ session.username }}' return render_template_string(tpl)
```

### 6. Настроить глобальные объекты `@app.context_processor`

Вы можете использовать декоратор context_processor объекта приложения для внедрения дополнительных глобальных объектов в движок. В следующем примере выполняется внедрение в глобальный домен шаблона._vendor_Переменная, значение которой_hubwiz_：

```python
@app.context_processor
def vendor_processor():
	return dict(vendor='hubwiz')
```

На данный момент мы можем использовать его прямо в шаблоне_vendor_Переменные:

```python
@app.route('/')
def v_index():
	tpl = 'powered by {{vendor}}' 
	return render_template_string(tpl)
```

Конечно, тот же метод можно использовать для внедрения глобальных функций. В следующем примере выполняется внедрение в глобальный домен шаблона._format_price_ функция:

```python
@app.context_processor
def utility_processor():
	def format_price(amount, currency=u'€'):
		return u'{0:.2f}{1}'.format(amount, currency)
	return dict(format_price=format_price)
```

### 7. Фильтр

Фильтры можно использовать в шаблонах_|_Чтобы изменить значение переменной. В следующем примере используется встроенный_title_Фильтр будет_name_Преобразуйте первую букву каждого слова в переменной в верхний регистр:

```python
tpl = '{{ name|title }}'print render_template_string(tpl,name='jimi hendrix') #Jimi Hendrix
```

**Каскад фильтров** ： Несколько фильтров можно соединить последовательно, образуя фильтрующий трубопровод. В следующем примере используются два фильтра по очереди для переменной имени,_scriptags_Фильтр используется для удаления тегов HTML в переменной имени,_title_Фильтр преобразует первую букву каждого слова входящей строки в верхний регистр:

```python
tpl = '{{ name|striptags|title }}'print render_template_string(tpl,name='<h1>jimi hendrix</h1>') #Jimi Hendrix
```

**Параметры фильтра**: Вы можете использовать круглые скобки для передачи дополнительных параметров фильтра. В следующем примере используется несколько членов переменной списка._join_К фильтрам подключаются:

```python
tpl = '{{ seq | join("-") }}' print render_template_string(tpl,seq=[1,2,3]) # 1-2-3
```

В Jinja2 фильтр на самом деле является функцией. Первый параметр используется для получения значения, переданного в предыдущей ссылке, а возвращаемое значение используется как первый параметр функции фильтра в последующей ссылке:

![10012631_oWIE.png](https://russianblogs.com/images/271/883f42246d4916787e1e8f27ab0009b7.png)  

Jinja2 имеет множество встроенных фильтров на официальном сайте[Страница документа](http://jinja.pocoo.org/docs/dev/templates/#builtin-filters) Вы можете понять детали.

### 8. Пользовательский фильтр.

Мы уже знаем, что фильтр на самом деле является функцией. В Flask вы можете использовать_Flask.template_filter_ Декоратор создает свой собственный фильтр. В следующем примере создается файл с именем_reverse_Фильтр разворота строки, который всегда переворачивает входную строку:

```python
@app.template_filter('reverse')def reverse_filter(s): return s[::-1]
```

В следующем примере показано, как вызвать наш самодельный фильтр:

```python
@app.route('/')def index(): return render_template_string('{{ greeting | reverse }}',greeting='Hello, Jinja2' )
```

Другой способ эквивалентного создания настраиваемого фильтра - добавить функцию фильтра к экземпляру приложения Flask._jinja_env_В словаре:

```python
def reverse_filter(s): return s[::-1]app.jinja_env.filters['reverse'] = reverse_filter
```

### 9. Блочный фильтр.

Фильтр блока может применять указанный фильтр ко всему содержимому блока:

```python
{% filter [filtername] %}...{% endfilter %}
```

В следующем примере используется содержимое шаблона в блоке фильтра._upper_Все фильтры переводятся в верхний регистр:

```python
tpl = ''' {% filter upper %} <p>Filter sections allow you to apply regular Jinja2 filters on a block  of template data. Just wrap the code in the special filter section</p> {% endfilter %} '''render_template_string(tpl)
```

Несколько фильтров также можно использовать в блочных фильтрах._каскад_. В следующем примере сначала используется содержимое блока фильтра._escape_ Отфильтруйте, чтобы убежать, а затем используйте_upper_Фильтр переводит его в верхний регистр:

```python
tpl = ''' {% filter escape | upper %} <p>Filter sections allow you to apply regular Jinja2 filters on a block  of template data. Just wrap the code in the special filter section</p> {% endfilter %} '''render_template_string(tpl)
```

### 10. Переменный риск в шаблоне

Базовая работа шаблонизатора приведена согласно_Неизвестно_Контекст данных в сочетании с шаблоном для создания строки HTML. Учитывая, что полученная строка HTML будет работать в браузере клиента, существует множество скрытых опасностей.

**XSS** : Если переменная шаблона получена в результате ввода пользователем, есть риск быть введенным злоумышленниками в сценарии для межсайтовых атак.

В приведенном ниже примере контекст данных_user_Содержимое взято из базы данных, а значение псевдонима / псевдонима может быть изменено пользователем. Злоумышленник добавил к своему нику скрипт. Когда любой пользователь заходит на личную страницу пользователя, он будет всплывать:

```python
user = {'id':123,'nickname':'haha<script>alert("xss vulnerable!")</script>'}tpl = '<h1>homepage of {{nickname}}</h1>'render_template_string(tpl,**user)
```

**Искажение страницы** : Еще одна незначительная, но более распространенная проблема заключается в том, что данные, отправленные пользователем, содержат символы HTML со специальным значением, например_<_、_>_、_&_Подождите. В следующем примере псевдоним пользователя выглядит как HTML-тег, который предотвращает отображение псевдонима на его личной странице:

```python
user = {'id':123,'nickname':'< IAMKING>'}tpl = '<h1>homepage of {{nickname}}</h1>'render_template_string(tpl,**user)
```

### 11. Экранирование переменных

Решение этих проблем - выполнить переменную_Побег_Операция, символы HTML со специальным значением в переменной представлены кодами объектов HTML. Например:_<IAMKING>_Будет преобразован в_<IAMKING>_。

**Автоматический выход** : Использование в шаблонах_autoescape_Теги могут включать или отключать функцию автоматического экранирования механизма шаблонов. Когда функция автоматического экранирования включена, механизм шаблонов автоматически выполняет операции экранирования для всех переменных в escape-блоке.

В следующем примере используйте_autoescape_Автоматическое экранирование включено для тегов:

```python
user = {'id':123,'nickname':'< IAMKING>'}tpl = ''' {% autoescape true %} <h1>homepage of <a href="/user/{{id}}">{{nickname}}</a></h1> {% endautoescape %} '''render_template_string(tpl,**user)
```

Но когда включено автоматическое экранирование, все переменные в escape-блоке будут экранированы, то есть эти переменные вообще не могут содержать символы HTML или их содержимое можно контролировать. Когда количество переменных велико, это приведет к ненужной потере производительности.

Мы можем использовать_safe_Фильтр помечает эти управляемые переменные как безопасные, и механизм рендеринга больше не будет их избегать. В следующем примере используйте_safe_Отмена ярлыка_id_Операция выхода переменных:

```python
user = {'id':123,'nickname':'< IAMKING>'}tpl = ''' {% autoescape true %}  <h1>homepage of <a href="/user/{{id | safe}}">{{nickname}}</a></h1> {% endautoescape %} '''render_template_string(tpl,**user)
```

**Вручную убежать** : Автоматическое экранирование соответствует экранированию переменной вручную. Метод заключается в использовании_escape_ Фильтр можно обозначить как_e_。

В следующем примере_nickname_Переменные экранируются вручную:

```python
user = {'id':123,'nickname':'< IAMKING>'}tpl = '<h1>homepage of <a href="/user/{{id}}">{{nickname | e }}</a></h1>'render_template_string(tpl,**user)
```

### 12. Структура петли

Предположим, у нас есть следующий набор пользовательских данных:

```python
data = [ {'name' : 'John','age' : 20,}, {'name' : 'Linda','age' : 21}, {'name' : 'Mary','age' : 30}, {'name' : 'Cook','age' : 40}]
```

Вы можете использовать структуру цикла для визуализации набора данных с использованием одного шаблона:

```python
{% for [loop condition] %}...{% endfor%}
```

В следующем примере создается по одному для каждого объекта в списке.

Тег :

```python
tpl = ''' <ul> {{% for user in users %}} <li>{{ user.name }}</li> {{% endfor %}} </ul> '''render_template_string(tpl,users=data)
```

**Итерационная фильтрация** : Цикл for в Jinja2 не может быть наполовину похожим на Python_Выход / перерыв_или же_Пропустить / продолжить_, Но поддерживает итерацию_Условная фильтрация_. В следующем примере шаблона элементы списка создаются только для пользователей старше 25 лет:

```python
tpl = ''' <ul> {{% for user in users if user.age > 25 %}} <li>{{ user.name }}</li> {{% endfor %}} </ul> '''render_template_string(tpl,users=data)
```

**Блок вывода по умолчанию** : Если цикл не выполняется хотя бы один раз (например, список пуст или отфильтрован), вы можете использовать_else_Блок генерирует вывод по умолчанию. Следующий образец шаблона будет выводиться, если ни один пользователь не соответствует_not found_：

```python
tpl = ''' <ul> {{% for user in users if user.age > 50 %}} <li>{{ user.name }}</li> {{% else %}} <li>not found!</li> {{% endfor %}} </ul> '''render_template_string(tpl,users=data)
```

### 13. Рекурсивный цикл

Некоторые данные являются рекурсивными данными с неопределенными уровнями, такими как файловая система, и в каталогах есть каталоги:

```python
/ приложение ------ каталог    /app.py ------ файл   / static ------ каталог       /main.css ------ файл       /jquery.min.css ------ файл   / templates ------ каталог       /user.html ------ файл
```

Соответствующее выражение данных см. В примере_tree_Объект. Поддержка структуры цикла Jinja2_Рекурсия_перечислить. Способ использования следующий:

##### 13.1 Использование_recursive_Цикл объявления ключевого слова_Рекурсия_цикл

```python
{% for item in data recursive}...{% endfor %}
```

##### 13.2 Внутри цикла используйте_loop()_Дочерний узел вызова функции

```python
{{ loop(item.children) }}
```

## 14. Специальные переменные в блоках цикла

В блоке цикла for Jinja2 предоставляет некоторые специальные переменные цикла:

**loop.index** : Порядковый номер текущего цикла, начиная с 1. Следующий пример выведет от 1 до 10:

```python
{% for i in range(10) %}{{ loop.index }}{% endfor %}
```

**loop.index0** : Номер выполняемого в данный момент цикла, начиная с 0.

**loop.revindex** : Обратный порядковый номер текущего выполняющегося цикла, начиная с 1. В следующем примере будет выведено 10 к 1:

```python
{% for i in range(10) %}{{ loop.revindex }}{% endfor %}
```

**loop.revindex0** : Обратный порядковый номер текущего выполняющегося цикла, начиная с 0

**loop.first** : Если текущее выполнение выполняется впервые в цикле, значение равно True. В следующем примере будет выведено True, False, False ...

```python
{% for i in range(10) %}{{ loop.index }}{% endfor %}
```

**loop.last** : Если уровень выполняется в последний раз в цикле, значение True

**loop.length** : Количество элементов в списке

**loop.cycle(*args)** : Взять значения в цикле из списка. Следующий пример выведет c1, c2, c3, c1, c2, c3 ...

```python
{% for i in range(10) %}{{ loop.cycle('c1','c2','c3') }}{% endfor %}
```

**loop.depth** : Глубина рекурсивного цикла, начиная с 1

**loop.depth0** : Глубина рекурсивного цикла, начиная с 0

### 15. Структура условий

В Jinja2 вы можете использовать_Блок условий_Задайте условия вывода содержимого шаблона. Только при соблюдении указанных условий содержимое шаблона в блоке условий будет отрисовано и выведено:

```python
{% if [condition] %}...{% endif %}
```

В следующем примере, только когда возраст пользователя не менее 18 лет, выводится контент, подходящий для просмотра взрослыми:

```python
data = {'name':'Obama',age:62}tpl = ''' {% if user.age >= 18 %} <div>some adult content...</div> {% endif %} '''render_template_string(tpl,user=data)
```

**elif** ： Можно добавлять и использовать для условного блока_elif_Добавьте несколько ветвей условного суждения:

```python
{% if [condition] %}...{% elif [condition2] %}...{% elif [condition3] %}...{% endif%}
```

В следующем примере, когда возраст пользователя превышает 60 лет, выводится программа здоровья, а когда пользователю старше 18 лет, но менее 60 лет, выводится программа для взрослых:

```python
data = {'name':'Obama',age:62}tpl = ''' {% if user.age >= 60%} <div>some health preserving content...</div> {% elif user.age >= 18 %} <div>some adult content...</div> {% endif %} '''render_template_string(tpl,user=data)
```

**else** : Когда все условия в блоке условий не выполняются, вы можете использовать_else_Добавьте блок вывода по умолчанию:

```python
{% if [condition] %}...{% elif [condition2] %}...{% else %}...{% endif %}
```

В следующем примере для несовершеннолетних выводятся мультипликационные программы:

```python
data = {'name':'Obama',age:62}tpl = ''' {% if user.age >= 60 %} <div>some health preserving content...</div> {% elif user.age >= 18 %} <div>some adult content...</div> {% else %} <div>some cartoon content...</div> {% endif %} '''render_template_string(tpl,user=data)
```

---

Reference:

### Общий синтаксис шаблона jinja2 в Python[](https://docs-python.ru/packages/modul-jinja2-python/sintaksis-shablona-jinja2/)
```jinja
{{ }}	# получить результат выражение, переменную или вызвать функцию и вывести значение в шаблоне
{# комментарий #}
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