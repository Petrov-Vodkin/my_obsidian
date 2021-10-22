2021-05-06 16:32
#virtmachine 
# Docker
`docker-compose` - управление группой контейнеров[](https://losst.ru/ispolzovanie-docker-dlya-chajnikov)[Установка и использование Docker Compose](https://www.8host.com/blog/ustanovka-i-ispolzovanie-docker-compose-na-ubuntu-14-04/ "Установка и использование Docker Compose") Установка [[Java]]
`docker-machine` - установка, настройка и управление `множества удаленных` Docker хостов
> _**docker**_ без запуска _**docker-desktop.exe**_
> После установки запустим приложение Docker Desktop и установим интеграцию Docker с дистрибутивом Linux (WSL 2) 
> Resources => WSL INTEGRATION => 'select debian||ubuntu||linuxOs'
### [[Основные команды Docker]]
### [[Шпаргалка с командами Docker]]
#### Экспорт и импорт физических образов Docker
```shell
docker save your_docker_image:latest > /usr/local/your_docker_image.tar$ docker load < /usr/local/your_docker_image.tar
```
```powershell
						`Запуск из под шелл`
net start com.docker.service	# запуск службы докер в win10
./DockerCli.exe -SwitchDaemon	# in C:\Program Files\Docker\Docker

1.  Run 'dockerd.exe' located in 'C:\Program Files\Docker\Docker\resources' .
2.  Now execute 'docker version' on a new powershell as administrator


```
#### Файлы [Dockerfile](https://routerus.com/how-to-build-docker-images-with-dockerfile/)
Best practice: 
 - `.dockerignore` (как gitignore)
 - команды `RUN` через && (apt update && apt upgrade) `снижаем кол-во слоёв`
 - отчистить `cache` atp
 - выбираем `minimal Linux` (Alpine 8mb ) Debian 200 mb
 - часто изменяемые слои\файлы (проэкт) `размещаем внизу DOCKER FILE`
 - прописывать `версии` ОС и приложух (МОЖНО ЧЕРЕЗ `ENV`)(+указывать репозиторий)
 - `Multi-stage` сборка
```DOCKER FILE
FROM ОБРАЗ | FROM ОБРАЗ:ТЭГ    # Задает базовый образ для последующих инструкций. Может встречаться несколько раз в одном Dockerfile, если необходимо собрать несколько образов за раз.
MAINTAINER имя    # Позволяет задать поле _Author_ сгенерированного образа
RUN команда | RUN ["исполняемый файл", "параметр1", "параметр2", ..]    # Запускает команду на основе текущего образа и фиксирует изменения в новом образе. Новый образ будет использован для исполнения последующих инструкций. Первый синтаксис подразумевает запуск команд в стандартной оболочке (bin\\sh -c)
CMD ["исполняемый файл", "параметр1", "параметр2"] | CMD ["параметр1", "параметр2"] | CMD команда параметр1 параметр2    # Предоставляет значения по умолчанию для запуска контейнера. Эти значения могут как включать исполняемый файл (варианты 1, 3), так и не включать его (вариант 2). В последнем случае запускаемая команда  должна быть задана с помощью инструкции `ENTRYPOINT`.
EXPOSE порт <порт...>    # Информирует Docker, что контейнер будет прослушивать указанные порты во время исполнения. Docker может использовать эту информацию, чтобы соединять контейнеры между собой используя связи. `EXPOSE` сам по себе не делает порт доступным из хостовой системы. Для того, чтобы открыть порты в хостовую систему следует использовать флаг `-p`.
ENV ключ значение    # Позволяет задавать переменные окружения. Эти переменные будут использованы при запуске контейнера из собранного образа. Могут быть просмотрены с помощью команды `docker inspect`, а также переопределены с помощью флага `--env` команды `docker run`.
ADD ОТКУДА <ОТКУДА...> КУДА    # Используется для добавления новых файлов, директорий или ссылок на удалённые файлы в файловую систему контейнера. Несколько ОТКУДА может быть передано одновременно, но в этом случае все адреса должны быть относительны для директории, из которой происходит сборка. Каждый вхождение ОТКУДА может содержатьодин или несколько символов подстановки, которые будут разрешены с использование функции языка Go filepath.Match. КУДА должен быть абсолютным путем внутри контейнера.
ENTRYPOINT ["исполняемый файл", "параметр1", "параметр2"] | ENTRYPOINT команда параметр1 параметр2    # позволяет сконфигурировать контейнер так, чтобы он запускался как исполняемый файл. В отличии от команды `CMD` значение не будет переопределено аргументами, переданными в команду `docker run`. Таким образом, аргументы из команды `docker run` будут переданы внутрь контейнера, т.е. `docker run ОБРАЗ -d` передаст -d исполняемому файлу.
VOLUME [ПУТЬ]    # создает точку монтирования с указанным именем и помечает её как содержащую подмонтированные разделы из хостовой системы или других контейнеров. Значение может быть задано как массив JSON, например, `VOLUME ["/var/log/"]`, так и как обычной строкой с одним или несколькими аргументами, например `VOLUME /var/log` или `VOLUME /var/log /var/db`
USER имя    # позволяет задавать имя пользователя или UID, который будет использован для запуска образа, а так же для любой из инструкций `RUN`
WORKDIR ПУТЬ    # задает рабочую директорию для команд `RUN`, `CMD` и `ENTRYPOINT`. Инструкция может быть использована несколько раз. Если ПУТЬ относителен, то он будет относительным для ПУТИ, заданным предыдущей инструкцией `WORKDIR`
```
```dockerfile
# nano/vim/micro Dockerfile - создайте txt document: Dockerfile
FROM ubuntu:18.04						# определяем базовое изображение
"Каждую команду выполнять отделбно НЕ чепез &&"
RUN apt-get update 		 			# обновит индекс apt and
RUN apt-get install -y redis-server # установит пакет «redis-server» and
RUN apt-get clean					# очистит кеш apt

EXPOSE 6379									# определяет порт, который прослушивает сервер Redis
CMD ["redis-server", "--protected-mode no"] # команды по умолчанию, которая будет выполняться при запуске контейнера.
# ____________________________________________________
FROM ubuntu:16.04

RUN apt-get -y update
RUN apt-get -y install apache2
RUN echo 'Hello World from Docker!' > /var/www/html/index.html

CMD ["/usr/sbin/apache2ctl", "-D","FOREGROUND"]
EXPOSE 80
```
Следующим шагом будет создание образа.
```shell
							" Создание образа"
docker build ПУТЬ | URL		# создает образ с помощью Dockerfile  
    		-t | --tag=""	# помечает созданный образ переданным названием (и, тэгом, если он будет передан)
    		--rm			#Удаляет промежуточные контейнеры после успешной сборки по умолчанию == true)
# создать фаил с именем Dockerfile + добавить в него нужные комманды
# из каталога, в котором находится Dockerfile:
docker build -t linuxize:tag . 	# Параметр `-t` указывает имя изображения ,: необязательно тег в формате «name:тег» 
								# . выбор docker фаила ЛОКАЛЬНО(в рабочей dir)
```
Рекомендации [по написанию файлов Docker](https://routerus.com/goto/https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) .

### [validate-yaml](https://onlineyamltools.com/validate-yaml)
_____________
#### Links
[[VirtualOS]] 
[Habr](https://habr.com/post/438796/)
[Losst](https://losst.ru/?s=Docker)
[Отчистка Docker](https://habr.com/ru/company/flant/blog/336654/)
[DOCKER COMMANDS (youtube)](https://github.com/adv4000/docker/blob/master/DOCKER%20COMMANDS.txt)
https://itnan.ru/post.php?c=1&p=534504 (запуск контейнеров без docker-desktop)



Running `sudo dockerd` in a seperate terminal solved the problem for me

systemctl enable docker # Type the command to enable docker while startup:
service docker start	# To start docker service: