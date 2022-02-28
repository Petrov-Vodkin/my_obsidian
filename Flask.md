2022-02-15 16:19
[__selfedu__](https://proproprogs.ru/flask) [quickstart_ru](https://flask-russian-docs.readthedocs.io/ru/latest/quickstart.html) [habr_учебник](https://habr.com/ru/post/346306/)
# Flask
### async `pip install flask[async]`
```python
@app.route("/get-data")
async def get_data():
    data = await async_db_query(...)
    return jsonify(data)
```
Рассмотрим пример использования aiohttp в маршруте Flask:

```python
urls = ['https://www.kennedyrecipes.com',
        'https://www.kennedyrecipes.com/breakfast/pancakes/',
        'https://www.kennedyrecipes.com/breakfast/honey_bran_muffins/']

# вспомогательные функции

async def fetch_url(session, url):
    """извлечь указанный URL-адрес, используя указанную сессию aiohttp."""
    response = await session.get(url)
    return {'url': response.url, 'status': response.status}


# маршруты

@app.route('/async_get_urls_v2')
async def async_get_urls_v2():
    """асинхронно извлекаем список url-адресов."""
    async with ClientSession() as session:
        tasks = []
        for url in urls:
			# Создаем несколько асинхронных задач
            task = asyncio.create_task(fetch_url(session, url))
            tasks.append(task)
		# Запускаем их одновременно
        sites = await asyncio.gather(*tasks)

    # Generate the HTML response
    response = '<h1>URLs:</h1>'
    for site in sites:
        response += f"<p>URL: {site['url']} --- Status Code: {site['status']}</p>"

    return response
```
__`Celery`__(`from celery import Celery`) - используется для выдачи фоновых заданий. При работе с Flask клиент запускается одновременно с приложением
```python
@celery.task
def my_background_task(arg1, arg2):
    # какое то задание 
    return result
```
______________________
### Архитектура
[[#app py]] [[#main py]] [[#config py]] [[#view.py]] [[#templates]] [[#models py]]
#### app.py
```python
from flask import Flask  
from config import Config

app = Flask(__name__)			#  создаём  приложение
app.config.from_object(Config) # подключаем конфиг
```
#### main.py
```python
from app import app  
import view				# !!!!!!!!!
  
if __name__ == "__main__":  
    app.run()
	
```
#### config.py
```python
class Config:  
    DEBUG = True
	
```
#### view.py
```python
from app import app  
from flask import render_template, g  
from FDataBase import FDataBase  
  
# app.config.update(dict(DATABASE=os.path.join(app.root_path, 'flsite.db')))  
  
@app.route('/')  # "привязка" ф_ии к урлу  
def index():  
    return render_template('index.html', menu=dbase.getMenu(), posts=dbase.getPostsAnonce())
```
#### models py 
```python
			''' базы '''
from app import db

ROLE_USER = 0
ROLE_ADMIN = 1

class User(db.Model):
    id = db.Column(db.Integer, primary_key = True)
    nickname = db.Column(db.String(64), index = True, unique = True)
    email = db.Column(db.String(120), index = True, unique = True)
    role = db.Column(db.SmallInteger, default = ROLE_USER)

    def __repr__(self):
        return '<User %r>' % (self.nickname)
	
```

#### templates
храняться html шаблоны
#### @app.route
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
''' создать config.py, сласс, добавить пар-ры конфигурации'''
from flask import Flask
from config import Config	# импортировать

app = Flask(__name__)
app.config.from_object(Config)
```
или так :)
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
__`создавать`__ начальную __`БД`__ с набором необходимых таблиц:
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

# Непосредственно создание таблиц	
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

### Функция make_response
самостоятельно определить некоторые параметры заголовка
```python
res_obj = make_response(res_body, status_code=200) # возвращает ссылку на объект ответа
#   res_body – передаваемое содержимое (контент);
#  status_code – код ответа сервера (по умолчанию 200).
```

```python
# EX1
'''отправить браузеру страницу в виде обычного текста. Для этого создадим следующий ответ:'''
@app.route("/")
def index():
    content = render_template('index.html', menu=menu, posts=[])
    res = make_response(content) # объект запроса
    res.headers['Content-Type'] = 'text/plain'# воспринимать контент как ОБЫЧНЫЙ текст (НЕ HTML)
    res.headers['Server'] = 'flasksite'
    return res
# EX2
'''загружаем изображение из подкаталога static/images и отдаем его браузеру, указывая, что принятые данные следует представлять в виде png изображения'''
@app.route("/")
def index():
    img = None
    with app.open_resource( app.root_path + "/static/images/ava.png", mode="rb") as f:
    	img = f.read()
 
    if img is None:
        return "None image"
 
    res = make_response(img)
    res.headers['Content-Type'] = 'image/png'
    return res
# EX3
'''можно вернуть страницу с определенным кодом, отличным от 200, например:'''
@app.route("/")
def index():
    res = make_response("<h1>Ошибка сервера</h1>", 500) # (response, status)
    return res

# (response, status, headers) or (response, headers) or (response, status)
@app.route("/")
def index(): # (response, status, headers)
    return "<h1>Main Page</h1>", 200, {'Content-Type': 'text/plain'}
```
### Создание 301 и 302 редиректов _`redirect(loc, stat)`_
```python
# 
@app.route('/transfer')
def transfer():
    return redirect(url_for('index'), 301)
# 301–страница перемещена на другой постоянный URL-адрес; 302–страница перемещена ВРЕМЕННО на другой URL-адрес.

```
### Перехват запросов
| `@app.`| действие|
|--|--|
|__`before_first_request`__ | выполняет функцию __до обработки `первого`__ запроса;|
|__`before_request`__ | выполняет функцию __до обработки `текущего`__ запроса;
|__`after_request`__ | выполняет функцию __`после` обработки запроса__ (при `отсутствии исключений` в обработчике запросов);
|__`teardown_request`__ | выполняет функцию __`после` обработки запроса__ 
```python
@app.before_first_request
def before_first_request():
    print("before_first_request() called")
 
@app.before_request
def before_request():
    print("before_request() called")
 
@app.after_request
def after_request(response):
    print("after_request() called")
    return response
 
@app.teardown_request
def teardown_request(response):
    print("teardown_request() called")
    return response
```
```python
dbase = None

@app.before_request
def before_request():
	"""Установление соединения с БД перед выполнением запроса"""
	global dbase
	db = get_db()
	dbase = FDataBase(db)

@app.teardown_appcontext
def close_db(error):
	'''Закрываем соединение с БД, если оно было установлено'''
	if hasattr(g, 'link_db'):
	g.link_db.close()
```
### Порядок работы с cookies
```python
set_cookie(key, value="", max_age=None)
# key – название куки;
# value – данные, которые сохраняются в cookies под указанным ключом;
# max_age – необязательно; предельное время хранения данных cookies в барузере клиента (в секундах). Если не указан, то куки пропадут при закрытии браузера.

@app.route("/login")
def login():
    log = ""
    if request.cookies.get('logged'):#если в запросе есть значение logget
        log = request.cookies.get('logged')
 	# формируем ответ сервера 
    res = make_response(f"<h1>Форма авторизации</h1>logged: {log}")
    res.set_cookie("logged", "yes")	#add куки к ответу res.set_cookie(key, vol)
    return res

'''Удаление cookies'''
@app.route("/logout")
def logout():
    res = make_response("Вы больше не авторизованы!</p>")
    res.set_cookie("logged", "", 0)#logged="", секунд хранения =0
    return res
```
### Порядок работы с сессиями (__`session`__)
сессии работают по похожему с cookies принципу: хранятся в браузере в виде особых кук сессий, шифруются с помощью ключа, короч - это __`зашифрованные куки`__
```python
'''Имеют ограничение в 4 Кб хранимой информации и перестают работать, если пользователь отключит cookies'''
app.config['SECRET_KEY'] = 'cb02820a3e9'
os.urandom(20).hex()	# сгенерировать ключь

				'''объект session работает по подобию словаря'''
@app.route("/")
def index():
    if 'visits' in session:
        session['visits'] = session.get('visits') + 1  # обновление данных сессии
    else:
        session['visits'] = 1  # запись данных в сессию
    return f"<h1>Main Page</h1>Число просмотров: {session['visits']}"
				'''передача сессионных данных браузеру происходит только при изменении состояния объекта session'''
session.modified = True # явно указываем Flask, что состояние сессии изменилось и ее нужно обновить в браузере
```
__`Время жизни сессии`__
По умолчанию куки сессии существуют, пока пользователь не закроет браузер. Но это поведение можно изменить и явно указать время жизни сессий в куках. Для этого в функции представления нужно указать:
```python
session.permanent = True
'''Тогда время жизни сессии устанавливается равным параметру'''
app.permanent_session_lifetime	# по умолчанию составляет 31 день
# Но, можно определить любое другое значение, например, так:
app.permanent_session_lifetime = datetime.timedelta(days=10)
```
### Регистрация пользователей и шифрование паролей
https://proproprogs.ru/flask/registraciya-polzovateley-i-shifrovanie-paroley
#### Шифрование паролей в БД
PBKDF2 (Password-Based Key Derivation Function) который доступен из пакета __`Werkzeug`__
```python
generate_password_hash() # выполняет кодирование строки данных по стандарту PBKDF2;
check_password_hash() # выполняет проверку указанных данных на соответствие хеша.
# EX
from werkzeug.security import generate_password_hash, check_password_hash

hash = generate_password_hash("12345")	# закодировать пароль
check_password_hash(hash, "12345")		# проверка (хэши должны быть равны)
```
### Авторизация пользователей на сайте через Flask-Login
```python
# pip install flask-login
from flask_login import LoginManager

login_manager = LoginManager(app)	# создать экземпляр


@login_manager.user_loader
'''Данная функция заносит в сессию информацию о зарегистрированном пользователе, используя определенные в классе методы. После этого сессионная информация будет присутствовать во всех запросах к серверу. Для ее обработки во Flask-Login определен специальный декоратор:'''
def load_user(user_id):
    print("load_user")
    return UserLogin().fromDB(user_id, dbase)

# Причем функции load_user этого декоратора будет передан идентификатор пользователя, который присутствует в сессии запроса. Фактически, это будет user_id, который возвращается методом get_id класса UserLogin.

# Зная этот идентификатор, мы можем обратиться к БД, прочитать данные этого пользователя и создать экземпляр класса UserLogin на основе прочитанных данных. Благодаря этому, у нас при каждом запросе в системе гарантированно будет объект класса UserLogin, с которым мы сможем в дальнейшем спокойно работать.
```
#### Начальная реализация авторизации
После создания экземпляра класса __`LoginManager`__  НЕОБХДИМО добавим в наш проект еще один файл `UserLogin.py`, в котором пропишем вспомогательный класс __`UserLogin`__:
```python
class UserLogin:
    def fromDB(self, user_id, db):
        self.__user = db.getUser(user_id) # получаем инфу о юзере  из БД
        return self
 
    def create(self, user):
        self.__user = user
        return self
 
    def is_authenticated(self):
		'''ф-я проверки авторизации юзера'''
        return True
 
    def is_active(self):
		'''проверка активности юзера ()'''
        return True
 
    def is_anonymous(self):
		'''определяет не авторизованных юзеров'''
        return False
 		
    def get_id(self):
		''' !!! Обязательно в виде STR'''
        return str(self.__user['id'])
```
#### login
```python
# EX
@login_manager.user_loader	
'''Загружаем юзера в экземплярЮзеров )) из БД'''
def load_user(user_id):
	print("load_user")
	return UserLogin().fromDB(user_id, dbase)


@app.route("/login", methods=["POST", "GET"])  
def login():  
    if current_user.is_authenticated:	# если залогинен, то
        return redirect(url_for('profile'))  
  
    if request.method == "POST":  
        user = dbase.getUserByEmail(request.form['email'])  # получаем юзера по мейлу(почта уникальна - есть проверочка)  
		if user and check_password_hash(user['psw'], request.form['psw']):  
        	userlogin = UserLogin().create(user)    # создаём экземпляр (юзера)  
			rm = True if request.form.get('remainme') else False # проверяем чекбокс (запомнить меня(login.html))  
			login_user(userlogin, remember=rm)      # заносим в сессию инфо текущем юзере  
			return redirect(request.args.get("next") or url_for("profile"))  
        # если не return - сообщение: "Неверная пара логин/пароль", "error"  
		flash("Неверная пара логин/пароль", "error")  
  
    return render_template("login.html", menu=dbase.getMenu(), title="Авторизация")
```
`login_user(userlogin, `remember=rm`)`      # заносим в сессию инфо текущем юзере, `remember=True` - "запомнить" пользователя
#### перенаправление `request.args.get("next")`
```python
'''Flask в строке запроса автоматически добавляет специальный параметр next, который ссылается на предыдущую страницу, с которой ушли
'''
def login():  
	# ....
	return redirect(request.args.get("next") or url_for("profile"))  

# HTML
# если есть next
<form {% if request.args.get('next') %}  
            action="{{ url_for('login', next=request.args.get('next')) }}"  
        {% else %}  
            action="{{ url_for('login') }}"  
        {% endif %}  
            method="post" class="form-contact" >
		# ....
</form>
```
#### `@login_required` доступ только автоизованных user's
```python
@app.route("/post/<alias>")
@login_required
def showPost(alias):
	pass
```
#### перенаправление(`не залогиненных`) на страницу логина
```python
login_manager.login_view = 'login' # атрибуту login_view присваиваем имя функции представления для формы авторизации
# формируем мгновенное сообщение
login_manager.login_message = "Авторизуйтесь для доступа к закрытым страницам"
login_manager.login_message_category = "success"

```
#### `current_user` класса UserLogin
```python
 			'''current_user - своего рода глобальная переменная, через которую можно обращаться к методам класса UserLogin'''
@app.route('/profile')
@login_required
def profile():
    return f"""<a href="{url_for('logout')}">Выйти из профиля</a>
                user info: {current_user.get_id()}"""
```
#### UserMixin
по умолчанию реализует __ОБЯЗАТЕЛЬНЫЕ__ методы: `is_authenticated`, `is_active`, `is_anonymous`
```python
from flask_login import UserMixin
 
class UserLogin(UserMixin):
    def fromDB(self, user_id, db):
		pass
```
#### logout
```python
@app.route('/logout')
@login_required
def logout():
    logout_user()	# очищает сессию и делает пользователя не авторизованным (from flask_login import logout_user)
    flash("Вы вышли из аккаунта", "success")
    return redirect(url_for('login'))
```
### Загрузка файлов на сервер [и сохранение в БД](https://proproprogs.ru/flask/zagruzka-faylov-na-server-i-sohranenie-v-bd)
`create table users (... avatar BLOB DEFAULT NULL);` -  __`BLOB`__ хранение бинарных данных
```python
MAX_CONTENT_LENGTH = 1024 * 1024 # == 1mb; константа конфигурации

# HTML
# обработчик upload(для загрузки)
# загрузка на сервер: enctype="multipart/form-data"
<form action="{{url_for('upload')}}" method="POST" enctype="multipart/form-data">
                            <input type="file" name="file">
                            <input type="submit" value="Загрузить">		
```
```python
							'''Загрузчик метод'''
@app.route('/upload', methods=["POST", "GET"])  
@login_required  
def upload():  
    if request.method == 'POST':  
        file = request.files['file']  
		# если загружен и verifyExt -верное расширение(png)(пишем метод в UserLogin)
        if file and current_user.verifyExt(file.filename):  
            try:  
                img = file.read()  
                res = dbase.updateUserAvatar(img, current_user.get_id())  
                if not res:  
                    flash("Ошибка обновления аватара", "error")  
                flash("Аватар обновлен", "success")  
            except FileNotFoundError as e:  
                flash("Ошибка чтения файла", "error")  
        else:  
            flash("Ошибка обновления аватара", "error")  
  
    return redirect(url_for('profile'))
		
						''' Загрузчик в базу'''
def updateUserAvatar(self, avatar, user_id):  
    if not avatar:	# если не получили аву - выход
        return False  
  
 	try:  
        binary = sqlite3.Binary(avatar)  
        self.__cur.execute(f"UPDATE users SET avatar = ? WHERE id = ?", (binary, user_id))  
        self.__db.commit()  
    except sqlite3.Error as e:  
        print("Ошибка обновления аватара в БД: "+str(e))  
        return False  
	return True
```
#### Отображение аватара
```python
@app.route('/userava')
@login_required
def userava():
    img = current_user.getAvatar(app)
    if not img:
        return ""
 
    h = make_response(img)
    h.headers['Content-Type'] = 'image/png'
    return h
```
## полезные расширения
### [Turbo-Flask](https://turbo-flask.readthedocs.io/en/latest/index.html) 
(`запуск задачи(корутины) в фоне`)
Динамически изменить тег. [EX](https://blog.miguelgrinberg.com/post/dynamically-update-your-flask-web-pages-using-turbo-flask)
### WTForms: `формамы` + `вальдация`
генерирует формы, проверяет их, наполняет начальной информацией, работать с reCaptcha, работа с ошибками и многое другое [официальном сайте](https://wtforms.readthedocs.io).
`pip install flask_wtf` + `pip install email-validator`
__`Создание класса формы`__

|				|						|
|---------------|-----------------------|
|   StringField | для работы с полем ввода |
|   PasswordField | для работы с полем ввода пароля |
|   BooleanField | для checkbox полей |
|   TextAreaField | для работы с вводом текста |
|   SelectField | для работы со списком |
|   SubmitField | для кнопки submit |

создадим forms.py, в котором будем определять все классы форм (ex: login.html) __`LoginForm`__:
```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField, BooleanField, PasswordField
from wtforms.validators import DataRequired, Email, Length
 
class LoginForm(FlaskForm):
    email = StringField("Email: ", validators=[Email()])
    psw = PasswordField("Пароль: ", validators=[DataRequired(), Length(min=4, max=100)])
    remember = BooleanField("Запомнить", default = False)
    submit = SubmitField("Войти")
```
#### Создание шаблона формы [WTForms](https://wtforms.readthedocs.io)
класс определен. Как им теперь пользоваться? в основном модуле программы выполним импорт:
```python
from forms import LoginForm

# в функции представления login создадим его экземпляр и передадим шаблону login.html:
@app.route("/login", methods=["POST", "GET"])
def login():
    form = LoginForm()
    return render_template("login.html", menu=dbase.getMenu(), title="Авторизация", form=form)
```
То есть, в шаблоне будет доступ к переменным этого класса через параметр form:
```html
{% extends 'base.html' %}
 
{% block content %}
{{ super() }}
{% for cat, msg in get_flashed_messages(True) %}
	<div class="flash {{cat}}">{{msg}}</div>
{% endfor %}
<form action="" method="post" class="form-contact">
	
	{{ form.hidden_tag() }} {#создает скрытое поле, содержащее токен, используемый для защиты формы от CSRF-атак#}
	                      {# form mail#}  
<p>{{ form.email.label() }}               
   {% if form.email.errors %}          {# если есть ошибки (До отображения)#}  
      {{ form.email(class="invalid") }}  {# отображаем с классом invalid#}  
      <span class="invalid-feedback">  
		{% for e in form.email.errors %}  
				{{ e }}  
			 {% endfor %}  
		  </span>  
		{% else %}  {{ form.email(size=32) }}  
   {% endif %}  
                     {# form password#}  
<p>{{ form.psw.label() }}                   
    {% if form.psw.errors %}  
      {{ form.psw(class="invalid") }}  
      <span class="invalid-feedback">  
		{% for e in form.psw.errors %}  
				{{ e }}  
			  {% endfor %}  
		  </span>  
		{% else %}  {{ form.psw() }}  
    {% endif %}
	{{ form.remember.label() }} {{ form.remember() }}
	{{ form.submit() }}
	<hr align=left width="300px">
	<a href="{{url_for('register')}}">Регистрация</a>
</form>
{% endblock %}
```
`form action=""` если для действия задана пустая строка, форма передается URL-адресу, находящемуся в данный момент в адресной строке, то есть URL-адресу, который визуализирует форму на странице
`<form action="" method="post" novalidate>` _novalidate_ - не применять проверку к полям в этой форме
`{{ form.email.label() }} {{ form.email(size=32) }} ` - form.email(size=32) max длинна
`{% for error in form.email.errors %} {# сообщение о ошибках #} <span style="color: red;">[{{ error }}]</span> {% endfor %}` -  сообщение о ошибках
#### Обработка ошибок во `Flask-WTF`
```python
class LoginForm(FlaskForm):
	# add выводимый текст [Email("Некорректный email")]
    email = StringField("Email: ", validators=[Email("Некорректный email")])
	# add выводимый текст message="Пароль должен быть от 4 до 100 символов"
    psw = PasswordField("Пароль: ", validators=[DataRequired(),
                                                Length(min=4, max=100, message="Пароль должен быть от 4 до 100 символов")])
    remember = BooleanField("Запомнить", default = False)
    submit = SubmitField("Войти")
```
Убираем повтор кода в html шаблоне (вывод ошибок)
```python
{# form #}  
{# кроме полей с именами 'csrf_token', 'remember' и 'submit'#}  
{% for field in form if field.name not in ['csrf_token', 'remember', 'submit'] -%}  
   <p> {{ field.label() }}  
   {% if field.errors %}  
            {{ field(class="invalid") }}  
    <span class="invalid-feedback">  
 {% for e in field.errors %}  
            {{ e }}  
            {% endfor %}  
    </span>  
 {% else %}  
            {{ field() }}  
    {% endif %}  
{% endfor %}
```
### `Blueprint`(эскизы)
"а ля микросервис" толко для проэкта (пакет с админкой, логином, товар, блог и тд), у каждой части своя `дир.я(имя_эскиза)` с обычной структурой(`templates/имя_эскиза` файлы шаблонов, `static` – файлы оформления)
```python
from flask import Blueprint

# создадим экземпляр этого класса:

admin = Blueprint('admin', __name__, template_folder='templates', static_folder='static')
# 'admin' – имя Blueprint, которое будет суффиксом ко всем именам методов, данного модуля;
# __name__ – имя исполняемого модуля, относительно которого будет искаться папка admin и соответствующие подкаталоги;
# template_folder – подкаталог для шаблонов данного Blueprint (необязательный параметр, при его отсутствии берется подкаталог шаблонов приложения);
# static_folder – подкаталог для статических файлов (необязательный параметр, при его отсутствии берется подкаталог static приложения).
```

После создания эскиза его нужно `зарегистрировать` в `основном файле` приложении или в `config`. Перейдем в файл flsite.py и выполним импорт переменной admin:

```python
from admin.admin import admin
# импортируем именно переменную, а не класс или функцию.

# выполним непосредственно регистрацию Blueprint:
app.register_blueprint(admin, url_prefix='/admin')# admin – ссылка на созданный Blueprint;
# url_prefix – префикс для всех URL модуля admin: урлы в эскизе начинаются с http://127.0.0.1:5000/admin

  
@admin.route('/')  
def index():  
    return "admin"
  
@admin.route('/login', methods=["POST", "GET"])  
def login():  
    if request.method == "POST":  
        if request.form['user'] == "admin" and request.form['psw'] == "12345":  
            login_admin()  
#'.index' - функцию-представления index следует брать для текущего Blueprint, а не глобальную из приложения  
		return redirect(url_for('.index'))  
        else:  
            flash("Неверная пара логин/пароль", "error")  
  
    return render_template('admin/login.html', title='Админ-панель')
```
#### blueprint подключение к БД
Добавим декораторы in admin.py :  `before_request` – перед выполнением запроса; `teardown_request` – после выполнения запроса.
```python
db = None
@admin.before_request
def before_request():
    """Установление соединения с БД перед выполнением запроса"""
    global db
    db = g.get('link_db')
 
@admin.teardown_request
def teardown_request(request):
    global db
    db = None
    return request
```
```python
@admin.route('/list-pubs')  
def listpubs():  
    						""" Список статей """  
	if not isLogged():  
        return redirect(url_for('.login'))  
  
    list = []  
    if db:  
        try:  
            cur = db.cursor()  
            cur.execute(f"SELECT title, text, url FROM posts")  
            list = cur.fetchall()  
        except sqlite3.Error as e:  
            print("Ошибка получения статей из БД " + str(e))  
  
    return render_template('admin/listpubs.html', title='Список статей', menu=menu, list=list)
```
#### Ссылка в css на blueprint
```python
# admin = Blueprint('admin', __name__, template_folder='templates', static_folder='static')
''' в шаблон HTML передаём "название"(шо в кавычках) '''
<a href = "{{url_for('admin')}}"> Adminka </a> >

```

### Flask-`SQLAlchemy`
`pip install Flask-SQLAlchemy`
https://proproprogs.ru/flask/flask-sqlalchemy-ustanovka-sozdanie-tablic-dobavlenie-zapisey
https://pythonru.com/biblioteki/sqlalchemy-v-flask
https://habr.com/ru/post/196810/
```python
# У нас есть парочка пунктов, которые мы добавим в файл конфигурации (файл config.py):  
import os

basedir = os.path.abspath(os.path.dirname(__file__))
SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(basedir, 'app.db')
SQLALCHEMY_MIGRATE_REPO = os.path.join(basedir, 'db_repository')
```
_______________
#### Links
[[Jinja]]
[git_self_progect](https://github.com/selfedu-rus/flasksite-21)
[официал док WTForms](https://wtforms.readthedocs.io)
