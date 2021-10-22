2020-12-03 17:02
[en doc](https://virtualenv.pypa.io/en/legacy/reference.html?highlight=prompt%20)
# [Venv](https://fixmypc.ru/post/sozdanie-virtualnogo-okruzheniia-v-python-3-s-venv-i-virtualenv/)
установка  ```pip install virtualenv```  в Python 3 уже должен быть модуль  
help:			```virtualenv --help```
### [Создадим виртуальное окружение:](https://stepik.org/lesson/25969/step/3?unit=196192)
```shell 
python3 -m venv selenium_env 		# v1 selenium_env - имя верт окружения
virtualenv -p python3.6 project/venv# v2(python run) venv - имя верт окружения
# -p параметр версии питона, если одна версия -p python версия не указываем
 ```
#### Деактивация:
```shell
deactivate # in windows
```
#### Активируем окружение:
```bash
$ source selenium_env/bin/activate
```
* просто укажите полный путь до файла 
				cd   \projectname\venv\Scripts\activate.ps1 -Powershell
				cd   \projectname\venv\Scripts\activate.bat -CMD
![[Pasted image 20201203173008.png]]	
* для CMD нужно указать путь до файла "venv\Scripts\deactivate.bat".

[Фиксируем пакеты в requirements.txt](https://stepik.org/lesson/193188/step/4?unit=167629)
```shell
# выполнить в терминале - сохранит все версии пакетов в requirements.txt
pip freeze > requirements.txt 
```
Как их оттуда достать? Попробуйте создать новое виртуальное окружение (если нужно, вернитесь в [модуль 1](https://stepik.org/lesson/25969/step/3?unit=196192) за инструкциями) и активировать. После чего выполните команду:
```shell
pip install -r requirements.txt
```

### Что находится в этих venv
-   **bin** – файлы, которые взаимодействуют с виртуальной средой;
-   **include** – С-заголовки, компилирующие пакеты Python;
-   **lib** – копия версии Python вместе с папкой «_site-packages_», в которой установлена каждая зависимость.
#### Links
[[PyTest]] [[Python]] [[Плагины PyTest]]
