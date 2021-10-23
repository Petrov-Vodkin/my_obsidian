2021-02-13 19:56
#tag
# Отправка сообщений и файлов в Telegram из консоли Linux(Ubuntu)
Для того чтобы отправлять сообщения и файлы из консоли Ubuntu ? очень просто.
```bash  
#      			Установка telegram cli ubuntu
wget https://itc-life.ru/wp-content/uploads/2016/09/telegram-cli_1.0.61_amd64.deb_.zip
unzip telegram-cli_1.0.6-1_amd64.deb_.zip
```
```bash
# 				Установка из deb пакета
sudo apt-get -y install libjansson4
sudo dpkg -i telegram-cli_1.0.6-1_amd64.deb
# 				Run / send
telegram-cli
contact_list
msg contact_name  “Привет”
```
_____________
#### Links
[[Telegram]]