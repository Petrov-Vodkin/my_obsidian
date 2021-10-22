2021-02-04 14:10
#безокон
# Headless Chrome
##### запуск без создания визуального окна браузер
[Скачиваем](https://www.google.ru/chrome/browser/canary.html) и устанавливаем Chrome Canary, который поддерживает все новые функции будущей версии Хрома. Не беспокойтесь, установка идет в отдельную папку по умолчанию и ваш родной текущий Хром браузер никак не пострадает. Сразу запоминаем или копируем путь установки.
Стандартно указываем путь к нашему драйверу хром
`System.setProperty("webdriver.chrome.driver", System.getProperty("user.dir") + "/vendors/chromedriver.exe");
`в данном случае, у меня драйвер лежит в папке проекта, в директории vendors.
Далее указываем хром проперти для использования headless режима
```python
ChromeOptions options = new ChromeOptions();
options.setBinary("C:\\Users\\admin\\AppData\\Local\\Google\\Chrome SxS\\Application\\chrome.exe");
options.addArguments("--headless");
```
в методе setBinary мы указываем путь к расположению нашего Chrome Canary, ну и гвоздем программы устанавливаем аргумент —headless, который говорит сам за себя.
далее, опять же по стандарту, просто создаем объект браузера
```python
driver = new ChromeDriver(options);
```
Можно запускать!
Единственный момент, который я сразу обнаружил — невидимое окно браузера всего размером 800х600, видно по скриншотам. Кому то может это и не важно, а у нас приложение меняет некоторые элементы в зависимости от размеров окна. Поэтому, нужный размер окна устанавливаем вот так
```python
options.addArguments("window-size=1800x900");
```
_____________
#### Links
[[Selenium]] [[PyTest]] [[Python]] [[Chrome]] [[Запуск pyTest]]