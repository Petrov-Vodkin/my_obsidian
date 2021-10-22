2021-02-01 13:30
#wifi #password #пароль
# WiFi
## Просмотр паролей ранее подключенных Wi-Fi сетей в командной строке
смотрим какие профили беспроводных сетей есть в системе
```bash
netsh wlan show profiles
```
вставляем имя профиля 
```bash
netsh wlan show profile NameProfile key=clear
```
В поле «**Содержимое ключа**» будет пароль.

#### Links
[[Windows]] [[PowerShell]]