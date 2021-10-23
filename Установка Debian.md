2021-03-30 14:12
#tag
# Установка Debian
[Live-образы установки](https://www.debian.org/CD/live/#choose_live)
- [Создание загрузочной ф-ки из-под Linux(используя только команды терминала)](https://habr.com/ru/post/53219/)
```bash
dmesg			# инф о подключенных USB
```
-  [Создание загрузочной ф-ки из=под Windows](https://poznyaev.ru/blog/linux/ustanovka-debian)
-  [4 способа записи на флешку](https://www.aitishnik.ru/linux/debian-install-usb.html)
#### Создание загрузочной флешки с помощью консольной утилиты dd (dataset definition).
```bash
sudo dd if=что_копировать of=куда_копировать параметры
sudo fdisk -l # определить название диска
						'example'
sudo dd if=/home/user/Загрузки/debian-10.3.0-amd64-netinst.iso of=/dev/sdc bs=4096
# bs - ускоряет запись
```
Для создания загрузочного USB-накопителя используйте [Universal USB Installer](https://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3).
_____________
#### Links
[[Debian]]