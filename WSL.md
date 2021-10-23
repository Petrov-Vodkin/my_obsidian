2021-05-07 15:35
#virtmachine 
# WSL
```PowerShell
wsl --distribution, -d <распределение>	# Запуск указанного распределения.
    --user, -u <имя_пользователя>		# Запуск от имени указанного пользователя.
	--list --verbose 	# статус (run||stop) версия || wsl -l -v
	--list --all 		# Список всех дистрибутивов, включая те, которые сейчас не используются. Они могут находиться в процессе установки, удаления или в неработающем состоянии.
	--list --running 	# Список всех распределений, выполняемых в данный момент.
'принудительно выключить или перезагрузить'
	--shutdown 			# выключить сразу все
	-t Distr_name		# выкл Distr_name || wsl --terminate	name
```
### Настройка дистрибутива по умолчанию
```Shell
# Дистрибутив по умолчанию WSL запускается при выполнении wsl в командной строке.
wsl -s <DistributionName> OR wsl --setdefault <DistributionName> # Задает для дистрибутив по умолчанию с помощью значения <DistributionName>
wsl -s Ubuntu			 # в качестве дистрибутива по умолчанию установит Ubuntu 
"Теперь при выполнении `wsl npm init` эта команда будет выполняться в Ubuntu"
wsl 					 # Now Если выполнить откроется сеанс Ubuntu.
								"Uninstall"
wslconfig.exe / u Ubuntu # чтобы удалить Ubuntu (замените Ubuntu на другое имя дистрибутива). Затем переустановите его обычным способом, если хотите.
```
`cd /mnt/c` для доступа к диску C:\\
### Установка/удаление дистрибутива
[Скачить](https://docs.microsoft.com/ru-ru/windows/wsl/install-manual)дистрибутив Linux
``` Powershell
Get-AppxPackage					# список установленных пакетов(смори PackageFullName)
Add-AppxPackage .\app_name.appx # установка `app_name`-имя/путь APPX-файла дистрибутива
						'Удаление'
Remover-AppxPackage	PackageFullName # удалить PackageFullName # или
lxrun /uninstall /full				# в запущенном wsl?
```
_**Проблемма при установки - ошибка файловой системы 12007**_
In the past, I used a tool [Destroy Windows 10 Spying](https://github.com/Nummer/Destroy-Windows-10-Spying) and it was blocking a bunch of Microsoft's addresses (Windows store app too) in the `C:\Windows\System32\drivers\etc\hosts` file and in the firewall. I removed those addresses from hosts and temporarily disabled the firewall and re-installed the terminal and it worked. I am suspecting it just couldn't download some default configuration or something like that which was hosted on the blocked IP and/or domain.

### Создание учетной записи / Сброс пароля 
https://docs.microsoft.com/ru-ru/windows/wsl/user-support
### Графика
В WSL не предусмотрена работа приложений с графическим интерфейсом. Тем не менее вы можете попробовать их установить и использовать. Чтобы запускать графические приложения в Linux, нужно скачать и установить в Windows программу **VcXsrv Windows X Server** ([https://sourceforge.net/projects/vcxsrv/](https://sourceforge.net/projects/vcxsrv/)). 
```bash
apt install prog_name
# Затем создайте файл в директории _root_:
cd /~  
vim .bash_login`
# впишите строку
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
Esc -> :wr -> :q	# сохраните запись

Теперь можете запустить графические программы
```

_____________
#### Links
[[VirtualOS]] 
[[Windows]]
[Руководство](https://docs.microsoft.com/ru-ru/windows/wsl/)
