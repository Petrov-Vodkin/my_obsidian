2022-02-15 16:19
[ru](https://pythonru.com/uroki/3-osnovy-flask) [quickstart_ru](https://flask-russian-docs.readthedocs.io/ru/latest/quickstart.html)
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

_____________
#### Links
[[Jinja]]
[git_self_progect](https://github.com/selfedu-rus/flasksite-17)
