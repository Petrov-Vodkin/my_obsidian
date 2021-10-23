2021-02-21 16:19
#cloud
# NextCloud
[Nextcloud + Apache Полная инструкция по установке, с регистрацией free (1year) домена](https://enk2x.ru/glstr/ncub18/#000133)
[Nextcloud + NGINX](https://enk2x.ru/glstr/nextcloud16ubuntunginxphpfpm/)
[Nextcloud + NGINX](https://www.dmosk.ru/miniinstruktions.php?mini=nextcloud-ubuntu)
### Подготовка системы
```bash
# root
apt-get install chrony 			    # синхронизирует время
timedatectl set-timezone Asia/Omsk  # выставляем нужный часовой пояс
systemctl enable chrony				# Разрешаем запуск демона chrony
```
### Установим ssh сервер
```bash
# root
apt update && apt install openssh-server
ip a 	# ip сетевой (пример inet 192.168.0.185)
vim /etc/ssh/sshd_config  	# приведём конфигурацию ssh к виду
							# Port 2222  
							# Protocol 2  
							# PermitRootLogin no  
systemctl restart sshd 		# 
```
### Фаервол
```bash
ufw status  # проверить статус фаервола (Состояние: неактивен)
# Добавим правила для ssh, http, https 
ufw allow 2222/tcp  
ufw allow 80/tcp  
ufw allow 443/tcp  
sudo ufw enable  # включим фаервол  ( Команда может разорвать существующие соединения ssh. Продолжить операцию (y|n)? y )
# Перезагружаем фаервол и ssh сервер если соединение не разорвалось, подключаемся по новому порту (2222):  
systemctl restart sshd 
systemctl restart ufw  
```
### Настроим статический ip адрес
```bash
	
```
_____________
#### Links
[[Debian]] [[Linux]] [[Сетевые команды]]