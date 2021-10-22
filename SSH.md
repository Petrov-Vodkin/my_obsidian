2021-02-20 13:39
#ssh
# SSH 
```bash
sudo apt install openssh-server # установить SSH 
					'Автозапуск'
sudo systemctl enable sshd		# добавить SSH в автозапуск
sudo systemctl disable sshd		# удалить службу из автозагрузки
					'Подключение'
ssh имя_пользователя@ip_адрес # 
ssh localhost	# поключиться к локальному серверу
ssh 0.0.0.0		# 
					`Настройка`
# сделать резавную копию файла конфигурации
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.factory-defaults
sudo vim /etc/ssh/sshd_config # 
Port 22 	 		 # меняем например на Port 2222
PermitRootLogin	     # значение no (запрет на вход от суперпользователя)
PubkeyAuthentication # авторизация по ключу yes

sudo systemctl restart ssh # перезапускаем ssh

```
### [SSH туннелирование](https://ru.wikibooks.org/wiki/SSH_%D1%82%D1%83%D0%BD%D0%BD%D0%B5%D0%BB%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)
_____________
#### Links
[[Linux]] [[Основные консольные команды Linux (Debian)]] [[Автозагрузка Debian]]