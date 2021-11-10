2021-05-06 16:32
[habr_post](https://habr.com/ru/company/ruvds/blog/439980/)
# Docker
`docker-compose` - управление группой контейнеров[](https://losst.ru/ispolzovanie-docker-dlya-chajnikov)[Установка и использование Docker Compose](https://www.8host.com/blog/ustanovka-i-ispolzovanie-docker-compose-na-ubuntu-14-04/ "Установка и использование Docker Compose") Установка [[Java]]
`docker-machine` - установка, настройка и управление `множества удаленных` Docker хостов
> _**docker**_ без запуска _**docker-desktop.exe**_
> После установки запустим приложение Docker Desktop и установим интеграцию Docker с дистрибутивом Linux (WSL 2) 
> Resources => WSL INTEGRATION => 'select debian||ubuntu||linuxOs'
#### [[Основные команды Docker]] |  [[Шпаргалка с командами Docker]] 
#### [[Container_docker]] | 
### Repository [докерхаб](https://hub.docker.com).
Заведите там аккаунт и создайте публичный репозиторий _`RepoName`_
```bash
docker login 	# введите логин и пароль. || docker login -u username -p pass
# Поставьте тэг в соответствии с именем репозитория:
docker tag go:v0.2 <Ваш username в Докерхаб>/RepoName:v0.2
# Теперь запушьте образ в хранилище:
docker push <Ваш username в Докерхаб>/RepoName:v0.2
```
### Экспорт и импорт физических образов Docker
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
_____________
#### Links
[[VirtualOS]] | [[Kubernetes]]
[Habr](https://habr.com/post/438796/) | [Losst](https://losst.ru/?s=Docker)
[Отчистка Docker](https://habr.com/ru/company/flant/blog/336654/) | [DOCKER COMMANDS (youtube)](https://github.com/adv4000/docker/blob/master/DOCKER%20COMMANDS.txt)

[ENTRYPOINT vs. CMD](https://habr.com/ru/company/southbridge/blog/329138/)
[Как собирать Docker контейнеры без Docker?](https://docs.gitlab.com/ee/ci/docker/using_kaniko.html)

[validate-yaml](https://onlineyamltools.com/validate-yaml)

__________
[запуск контейнеров без docker-desktop](https://itnan.ru/post.php?c=1&p=534504)
```bash
# Running `sudo dockerd` in a seperate terminal solved the problem for me
systemctl enable docker # Type the command to enable docker while startup:
service docker start	# To start docker service:
```