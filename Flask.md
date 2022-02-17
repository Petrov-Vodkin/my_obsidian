2022-02-15 16:19
[__selfedu__](https://proproprogs.ru/flask) [quickstart_ru](https://flask-russian-docs.readthedocs.io/ru/latest/quickstart.html)
# Flask
```python
# EX отладка урлов
with app.test_request_context():    # создание конт-та запросов 4отладки  
    print(url_for('about'))  
    print(url_for('telephone'))
```
По умолчанию переменная часть в URL-адресе принимает любую строку без косой черты `/`. Можно указать другой конвертер, используя схему: `<converter:name>`, где `converter` - это один из конвертеров (указанных ниже), а - `name` - имя переменной части маршрута.
```python
# `string`: принимает любой текст без косой черты (по умолчанию);
# `int`: принимает целые числа;
# `float`: как `int`, но для значений с плавающей запятой;
# `path`: принимает любой текст, но также принимает косые черты;
# `any`: соответствует одному из предоставленных элементов;
# `uuid`: принимает строки `UUID`.

@app.route('/profile/<path:username>') # формирование урла из переменнй  
# <username>  передаётся из браузерной строки  
def profile(username):  
    return f"Пользователь: {username}"
```
### CSS, JavaScript, шрифты и так далее[.](https://pythonru.com/uroki/9-rabota-so-staticheskimi-fajlami-vo-flask)
по умолчанию Flask ищет статические файлы в папке `static`
```python
# изменить директорию статич. файлов на static_dir 
app = Flask(__name__, static_folder="static_dir")
```
`url_for()` - позволяет генерировать URL-адрес по имени функции-обработчика
#### Подключить оформление страницы `url_for('static', filename='css/styles.css')`
```python
					# подключение осуществляется в файле HTML
# href(ссылка) url_for('static',                    - директория (дефолт)
#                       filename='css/styles.css    - поддирект/фаил
<head>
<link type="text/css" href="{{ url_for('static', filename='css/styles.css') }}" rel="stylesheet"/>
</head>
```
#### добавить Links
```python
# dict название: ссылка
menu = [{"name": "Установка", "url": "install-flask"},  
 {"name": "Первое приложение", "url": "first-app"},  
 {"name": "Обратная связь", "url": "contact"}]

# in HTML (перебор словаря menu в цикле )
{% for m in menu %}
	<li><a href="{{m.url}}">{{m.name}}</a></li> {# links  #}
{% endfor %}

```
#### Работа с формой (form)
```python
# in action указывается URL(ф-я обработчик), который должен принимать данные от формы
# method определяет способ передачи данных (get || post)
<form action="/contact" method="post" class="form-contact">
<p><label>Имя: </label> <input type="text" name="username" value="" requied />
<p><label>Email: </label> <input type="text" name="email" value="" requied />
<p><label>Сообщение:</label>
<p><textarea name="message" rows=7 cols=40></textarea>
<p><input type="submit" value="Отправить" />
</form>

# в обработчике указываем метод, которым будут приниматься данные methods=["POST"]
@app.route('/contact', methods=['POST', 'GET'])  
def contact():  
    if request.method == 'POST':  
        print(request.form)  
  
    return render_template('contact.html', title='Обратная связь', menu=menu)

```
#### Мгновенные сообщения - flash, get_flashed_messages
`flash` и `get_flashed_messages` используют механизм сессий, чтобы отображать сообщения только один раз и при обновлении страницы более его не показывать.  `secret_key` - необходим для работы с сессиями
```python
app.config['SECRET_KEY'] = 'fdgdfgdfggf786hfg6hfg6h7f' # EX 
```

_`flash()` – формирование сообщения пользователю_
```python
				# in Flask project
if len(request.form['username']) > 2:  
    flash('Сообщение отправлено', category='success')  # category - для применения 'нужной' css
else:  
    flash('Ошибка отправки', category='error')
```
_`get_flashed_messages()` – обработка сформированных сообщений в шаблоне документа._
```python
					# в HTML
{% for cat, msg in get_flashed_messages(True) %}  # True - ф-я return cat, msg (не только msg)
    <div class="flash {{cat}}">{{msg}}</div>  
{% endfor %}

					# CSS 
.flash {padding: 10px;}  
	.flash.success {  
	   border: 1px solid #21DB56;  
	   background: #AEFFC5;  
	}  
	.flash.error {  
	   border: 1px solid #FF4343;  
	   background: #FF9C9C;  
	}
```
#### errorhandler, функции redirect и abort
__Обработка ошибок__
```python
# 				EX обработчик для кода 404			
@app.errorhandler(404)    # указать кода ответа(404), с которым будет ассоциирована функция представления:
def pageNotFount(error):
	# return шаблон + код 404 (т.к def 200)
    return render_template('page404.html', title="Страница не найдена", menu=menu), 404

#				 in HTML
{% extends 'base.html' %}       {# расширяем  base.html #}  
  
{% block content %}             {# в этот блок #}  
    {{ super() }}                   {# вызываем блок контент #}  
    <p>Страница не найдена, вернуться на <a href="{{url_for('index')}}">главную</a> страницу.  
{% endblock %}
```
#### Перенаправление запроса
__перенаправление `уже зал-го юзера на стр профайла`__
```python
@app.route('/login', methods=['POST', 'GET'])  
def login():  
    if 'userLogged' in session:  
        return redirect(url_for('profile', username=session['userLogged']))  
    elif request.method == 'POST' and request.form['username'] == "selfedu" and request.form['psw'] == "123":  
        session['userLogged'] = request.form['username']  
        return redirect(url_for('profile', username=session['userLogged']))  
  
    return render_template('login.html', title="Авторизация", menu=menu)
```
__проверка `аутентификации`__
```python
@app.route('/profile/<path:username>') # формирование урла из переменнй  
# <username>  передаётся из браузерной строки  
def profile(username):  
    # если юзер не залогинелся или имя юзера в сессии != имени переданным из урла,   
 if 'userLogged' not in session or session['userLogged'] != username:  
        abort(401)  
  	# то ПРЕРЫВАЕМ запрос с кодом 4-1
    return f"Пользователь: {username}"
```
### Создание БД, установление и разрыв соединения при запросах
#### Config
```python
DATABASE = '/tmp/flsite.db'  
DEBUG = True  
SECRET_KEY = 'fdgfh78@#5?>gfhf89dx,v06k'  
USERNAME = 'admin'  
PASSWORD = '123'  
  
app = Flask(__name__)               # "создаём приложение"  
app.config.from_object(__name__)    # загружаем из модуля (__наме__ == из текущего модуля)  
# app.config['SECRET_KEY'] = 'wjhebwec8382bckjas'  
# переопределим путь к БД: app.root_path == dir_приложения, flsite.db == наме файла БД  
app.config.update(dict(DATABASE=os.path.join(app.root_path, 'flsite.db')))
```
#### Создание БД
функция для установления __`соединения`__ с БД:
```python
def connect_db():
	# app.config['DATABASE'] - путь к БД из конфигурации
    conn = sqlite3.connect(app.config['DATABASE'])
    conn.row_factory = sqlite3.Row # записи из БД будут представленны в виде словаря
    return conn # возвращает соединение 
```
создавать начальную БД с набором необходимых таблиц:
```python
def create_db():
    """Вспомогательная функция для создания таблиц БД(без запуска приложения(flask(сайта)))"""
    db = connect_db()
	# открываем заготовленный sql_скрипт для создания БД
    with app.open_resource('sq_db.sql', mode='r') as f:
		# курсор.запуск_скриптов(сам_скрипт)
        db.cursor().executescript(f.read())
    db.commit()	# запись измениеий в БД
    db.close()	# звкрываем соединение
```
```python
# можно запустить в py_консол 
from flsite import create_db
create_db()
```
```python
def get_db():  
    """Соединение с БД, если оно еще не установлено"""  
# здесь используем объект g контекста приложения, которое создается в момент поступления запроса  
	if not hasattr(g, 'link_db'):   # если у объекта g нет св-ва link_db        
		g.link_db = connect_db()    # создаём св-во равное connect_db()   
	return g.link_db                # иначе вернём имеющиеся
```
```python
# teardown_appcontext - "срабатывает" когда уничтожается контекст приложения(в момент завершения обработки запроса)  
@app.teardown_appcontext  
def close_db(error):  
    """Закрываем соединение с БД, если оно было установлено"""  
 	if hasattr(g, 'link_db'):  
        g.link_db.close()
```
#### Добавление и отображение статей из БД



_____________
#### Links
[[Jinja]]
[git_self_progect](https://github.com/selfedu-rus/flasksite-17) + [web](https://proproprogs.ru/flask)
