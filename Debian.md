2021-02-08 16:38
#linuxOs
# Debian
[[Установка Debian]] | [[i3wm]] | [[Автозагрузка Debian]]
[[Основные консольные команды Linux (Debian)]]
#### После "чистой" установки:
```bash
apt list --installed					# установленные пакеты
dpkg --get-selections 					# установленные пакеты
dpkg --get-selections > ~/package.txt   # вывести эту информацию в файл

sudo apt install synaptic # Можно поставить  Synaptic(Графический пакетный менеджер)
```
 ***Редактируем*** список репозиториев:`nano /etc/apt/sources.list` [[Репозиторий]]
 ```bash
 									'Users'
 apt-get update && apt-get upgrade -y
 apt-get install sudo 			# установим sudo(root права обычному пользователю)
 adduser user_name				# создадим пользователя
 usermod -aG sudo user_name  	# присваеваем user права su(добавляем юзера в группу sudo)
 ```
 **Настроим разрешение монирота**
```bash
sudo xrandr										# возможное разрешение 
sudo xrandr --output NameMonitor --mode 800x600 # применим подходящее разрешение 									
```
Настроим автозапуск (при запуске граф процессора(`xorg`) i3wm)
```bash
echo 'exec i3' >> ~/.xinitrc #  создадим и отредактируем файл ~/.xinitrc, записав в него текст “exec i3”
# можно автоматом выставлять расширение (из xrand)
```
### Возможные проблеммы / решения
X server -  [startx ошибка без root?](https://qastack.ru/unix/101930/how-to-run-startx-as-non-root) ```rm ~/.Xauthority; startx```
_____________
#### Links
[[Linux]] 